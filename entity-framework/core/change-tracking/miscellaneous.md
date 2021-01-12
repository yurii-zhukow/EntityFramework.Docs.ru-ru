---
title: Дополнительные функции Отслеживание изменений — EF Core
description: Различные функции и сценарии, включающие отслеживание изменений EF Core
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/miscellaneous
ms.openlocfilehash: db1e32948b2a60ad1b85e300bbbccd54d49a84e5
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129646"
---
# <a name="additional-change-tracking-features"></a><span data-ttu-id="7c054-103">Дополнительные функции Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="7c054-103">Additional Change Tracking Features</span></span>

<span data-ttu-id="7c054-104">В этом документе рассматриваются различные функции и сценарии, включающие отслеживание изменений.</span><span class="sxs-lookup"><span data-stu-id="7c054-104">This document covers miscellaneous features and scenarios involving change tracking.</span></span>

> [!TIP]
> <span data-ttu-id="7c054-105">В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны.</span><span class="sxs-lookup"><span data-stu-id="7c054-105">This document assumes that entity states and the basics of EF Core change tracking are understood.</span></span> <span data-ttu-id="7c054-106">Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="7c054-106">See [Change Tracking in EF Core](xref:core/change-tracking/index) for more information on these topics.</span></span>

> [!TIP]
> <span data-ttu-id="7c054-107">Вы можете запустить и отладить весь код в этом документе, [загрузив пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/AdditionalChangeTrackingFeatures).</span><span class="sxs-lookup"><span data-stu-id="7c054-107">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/AdditionalChangeTrackingFeatures).</span></span>

## <a name="add-verses-addasync"></a><span data-ttu-id="7c054-108">Добавление обратных метод AddAsync</span><span class="sxs-lookup"><span data-stu-id="7c054-108">Add verses AddAsync</span></span>

<span data-ttu-id="7c054-109">Entity Framework Core (EF Core) предоставляет асинхронные методы всякий раз, когда использование этого метода может привести к взаимодействию с базой данных.</span><span class="sxs-lookup"><span data-stu-id="7c054-109">Entity Framework Core (EF Core) provides async methods whenever using that method may result in a database interaction.</span></span> <span data-ttu-id="7c054-110">Кроме того, существуют синхронные методы, позволяющие избежать издержек при использовании баз данных, не поддерживающих высокий уровень производительности асинхронного доступа.</span><span class="sxs-lookup"><span data-stu-id="7c054-110">Synchronous methods are also provided to avoid overhead when using databases that do not support high performance asynchronous access.</span></span>

<span data-ttu-id="7c054-111"><xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.Add%2A?displayProperty=nameWithType> обычно не обращаются к базе данных, так как эти методы изначально просто начинают отслеживать сущности.</span><span class="sxs-lookup"><span data-stu-id="7c054-111"><xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.DbSet%601.Add%2A?displayProperty=nameWithType> do not normally access the database, since these methods inherently just start tracking entities.</span></span> <span data-ttu-id="7c054-112">Однако некоторые формы создания значений _могут_ получить доступ к базе данных, чтобы создать значение ключа.</span><span class="sxs-lookup"><span data-stu-id="7c054-112">However, some forms of value generation _may_ access the database in order to generate a key value.</span></span> <span data-ttu-id="7c054-113">Единственным генератором значений, который делает это и поставляется с EF Core, является <xref:Microsoft.EntityFrameworkCore.ValueGeneration.HiLoValueGenerator%601> .</span><span class="sxs-lookup"><span data-stu-id="7c054-113">The only value generator that does this and ships with EF Core is <xref:Microsoft.EntityFrameworkCore.ValueGeneration.HiLoValueGenerator%601>.</span></span> <span data-ttu-id="7c054-114">Использование этого генератора является редким; Он никогда не настраивается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7c054-114">Using this generator is uncommon; it is never configured by default.</span></span> <span data-ttu-id="7c054-115">Это означает, что большинству приложений следует использовать `Add` , а не `AddAsync` .</span><span class="sxs-lookup"><span data-stu-id="7c054-115">This means that the vast majority of applications should use `Add`, and not `AddAsync`.</span></span>

<span data-ttu-id="7c054-116">Другие аналогичные методы, такие как `Update` , `Attach` и, `Remove` не имеют асинхронных перегрузок, так как они никогда не создают новые значения ключей и, следовательно, никогда не нуждаются в доступе к базе данных.</span><span class="sxs-lookup"><span data-stu-id="7c054-116">Other similar methods like `Update`, `Attach`, and `Remove` do not have async overloads because they never generate new key values, and hence never need to access the database.</span></span>

## <a name="addrange-updaterange-attachrange-and-removerange"></a><span data-ttu-id="7c054-117">AddRange, Упдатеранже, Аттачранже и Ремоверанже</span><span class="sxs-lookup"><span data-stu-id="7c054-117">AddRange, UpdateRange, AttachRange, and RemoveRange</span></span>

<span data-ttu-id="7c054-118"><xref:Microsoft.EntityFrameworkCore.DbSet%601> и <xref:Microsoft.EntityFrameworkCore.DbContext> предоставляют альтернативные версии `Add` , `Update` , `Attach` и `Remove` , которые принимают несколько экземпляров в одном вызове.</span><span class="sxs-lookup"><span data-stu-id="7c054-118"><xref:Microsoft.EntityFrameworkCore.DbSet%601> and <xref:Microsoft.EntityFrameworkCore.DbContext> provide alternate versions of `Add`, `Update`, `Attach`, and `Remove` that accept multiple instances in a single call.</span></span> <span data-ttu-id="7c054-119">Эти методы называются `AddRange` , `UpdateRange` , `AttachRange` и `RemoveRange` соответственно.</span><span class="sxs-lookup"><span data-stu-id="7c054-119">These methods are called `AddRange`, `UpdateRange`, `AttachRange`, and `RemoveRange` respectively.</span></span>

<span data-ttu-id="7c054-120">Эти методы предоставляются для удобства.</span><span class="sxs-lookup"><span data-stu-id="7c054-120">These methods are provided as a convenience.</span></span> <span data-ttu-id="7c054-121">Использование метода Range имеет те же функциональные возможности, что и несколько вызовов эквивалентного метода, не являющегося диапазоном.</span><span class="sxs-lookup"><span data-stu-id="7c054-121">Using a "range" method has the same functionality as multiple calls to the equivalent non-range method.</span></span> <span data-ttu-id="7c054-122">Между двумя подходами нет значительной разницы в производительности.</span><span class="sxs-lookup"><span data-stu-id="7c054-122">There is no significant performance difference between the two approaches.</span></span>

> [!NOTE]
> <span data-ttu-id="7c054-123">Это отличается от EF6, где AddRange и добавляют оба автоматически называемые DetectChanges, но вызов Add несколько раз вызвал многократное обращение к DetectChanges, а не один раз.</span><span class="sxs-lookup"><span data-stu-id="7c054-123">This is different from EF6, where AddRange and Add both automatically called DetectChanges, but calling Add multiple times caused DetectChanges to be called multiple times instead of once.</span></span> <span data-ttu-id="7c054-124">Это стало более эффективным в EF6.</span><span class="sxs-lookup"><span data-stu-id="7c054-124">This made AddRange more efficient in EF6.</span></span> <span data-ttu-id="7c054-125">В EF Core ни один из этих методов не вызывает DetectChanges автоматически.</span><span class="sxs-lookup"><span data-stu-id="7c054-125">In EF Core, neither of these methods automatically call DetectChanges.</span></span>

## <a name="dbcontext-verses-dbset-methods"></a><span data-ttu-id="7c054-126">DbContext DbSet методы обратной кинематики</span><span class="sxs-lookup"><span data-stu-id="7c054-126">DbContext verses DbSet methods</span></span>

<span data-ttu-id="7c054-127">Многие методы, включая `Add` , `Update` , `Attach` и `Remove` , имеют реализации как в, так <xref:Microsoft.EntityFrameworkCore.DbSet%601> и в <xref:Microsoft.EntityFrameworkCore.DbContext> .</span><span class="sxs-lookup"><span data-stu-id="7c054-127">Many methods, including `Add`, `Update`, `Attach`, and `Remove`, have implementations on both <xref:Microsoft.EntityFrameworkCore.DbSet%601> and <xref:Microsoft.EntityFrameworkCore.DbContext>.</span></span> <span data-ttu-id="7c054-128">Эти методы имеют _точно такое же поведение_ для обычных типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="7c054-128">These methods have _exactly the same behavior_ for normal entity types.</span></span> <span data-ttu-id="7c054-129">Это обусловлено тем, что тип CLR сущности сопоставлен с одним и только одним типом сущности в модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c054-129">This is because the CLR type of the entity is mapped onto one and only one entity type in the EF Core model.</span></span> <span data-ttu-id="7c054-130">Таким образом, тип CLR полностью определяет, где сущность помещается в модели, поэтому DbSet может быть определен неявно.</span><span class="sxs-lookup"><span data-stu-id="7c054-130">Therefore, the CLR type fully defines where the entity fits in the model, and so the DbSet to use can be determined implicitly.</span></span>

<span data-ttu-id="7c054-131">Исключением из этого правила является использование типов сущностей с общим типом, которые были введены в EF Core 5,0, в первую очередь для сущностей объединения «многие ко многим».</span><span class="sxs-lookup"><span data-stu-id="7c054-131">The exception to this rule is when using shared-type entity types, which were introduced in EF Core 5.0, primarily for many-to-many join entities.</span></span> <span data-ttu-id="7c054-132">При использовании типа сущности общего типа необходимо сначала создать DbSet для используемого типа модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c054-132">When using a shared-type entity type, a DbSet must first be created for the EF Core model type that is being used.</span></span> <span data-ttu-id="7c054-133">Такие методы `Add` , как,, `Update` и, `Attach` `Remove` можно использовать в DbSet без какой-либо неоднозначности, в которой используется тип модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c054-133">Methods like `Add`, `Update`, `Attach`, and `Remove` can then be used on the DbSet without any ambiguity as to which EF Core model type is being used.</span></span>

<span data-ttu-id="7c054-134">Типы сущностей общего типа используются по умолчанию для сущностей JOIN в связях «многие ко многим».</span><span class="sxs-lookup"><span data-stu-id="7c054-134">Shared-type entity types are used by default for the join entities in many-to-many relationships.</span></span> <span data-ttu-id="7c054-135">Тип сущности совместно используемого типа также может быть явно настроен для использования в связи «многие ко многим».</span><span class="sxs-lookup"><span data-stu-id="7c054-135">A shared-type entity type can also be explicitly configured for use in a many-to-many relationship.</span></span> <span data-ttu-id="7c054-136">Например, приведенный ниже код настраивается `Dictionary<string, int>` как тип сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="7c054-136">For example, the code below configures `Dictionary<string, int>` as a join entity type:</span></span>

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

<span data-ttu-id="7c054-137">[Изменение внешних ключей и переходов](xref:core/change-tracking/relationship-changes) показывает, как связать две сущности путем отслеживания нового экземпляра сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="7c054-137">[Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes) shows how to associate two entities by tracking a new join entity instance.</span></span> <span data-ttu-id="7c054-138">Приведенный ниже код делает это для `Dictionary<string, int>` типа сущности общего типа, используемого для сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="7c054-138">The code below does this for the `Dictionary<string, int>` shared-type entity type used for the join entity:</span></span>

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
[!code-csharp[DbContext_verses_DbSet_methods_1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/Samples.cs?name=DbContext_verses_DbSet_methods_1)]

<span data-ttu-id="7c054-139">Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.DbContext.Set%60%601(System.String)?displayProperty=nameWithType> используется для создания DbSet для `PostTag` типа сущности.</span><span class="sxs-lookup"><span data-stu-id="7c054-139">Notice that <xref:Microsoft.EntityFrameworkCore.DbContext.Set%60%601(System.String)?displayProperty=nameWithType> is used to create a DbSet for the `PostTag` entity type.</span></span> <span data-ttu-id="7c054-140">Этот DbSet можно затем использовать для вызова `Add` с новым экземпляром сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="7c054-140">This DbSet can then be used to call `Add` with the new join entity instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c054-141">Тип CLR, используемый для объединения типов сущностей по соглашению, может измениться в будущих выпусках для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="7c054-141">The CLR type used for join entity types by convention may change in future releases to improve performance.</span></span> <span data-ttu-id="7c054-142">Не зависят от какого-либо конкретного типа сущности JOIN, если только она не была явно настроена в приведенном `Dictionary<string, int>` выше коде.</span><span class="sxs-lookup"><span data-stu-id="7c054-142">Do not depend on any specific join entity type unless it has been explicitly configured as is done for `Dictionary<string, int>` in the code above.</span></span>

## <a name="property-verses-field-access"></a><span data-ttu-id="7c054-143">Доступ к полю с обратными знаками свойства</span><span class="sxs-lookup"><span data-stu-id="7c054-143">Property verses field access</span></span>

<span data-ttu-id="7c054-144">Начиная с EF Core 3,0, доступ к свойствам сущности по умолчанию использует резервное поле свойства.</span><span class="sxs-lookup"><span data-stu-id="7c054-144">Starting with EF Core 3.0, access to entity properties uses the backing field of the property by default.</span></span> <span data-ttu-id="7c054-145">Это эффективно и позволяет избежать запуска побочных эффектов при вызове методов получения и задания свойств.</span><span class="sxs-lookup"><span data-stu-id="7c054-145">This is efficient and avoids triggering side effects from calling property getters and setters.</span></span> <span data-ttu-id="7c054-146">Например, вот как отложенная загрузка позволяет избежать срабатывания бесконечных циклов.</span><span class="sxs-lookup"><span data-stu-id="7c054-146">For example, this is how lazy-loading is able to avoid triggering infinite loops.</span></span> <span data-ttu-id="7c054-147">Дополнительные сведения о настройке резервных полей в модели см. в разделе [резервные поля](xref:core/modeling/backing-field) .</span><span class="sxs-lookup"><span data-stu-id="7c054-147">See [Backing Fields](xref:core/modeling/backing-field) for more information on configuring backing fields in the model.</span></span>

<span data-ttu-id="7c054-148">Иногда может быть целесообразно EF Core создавать побочные эффекты при изменении значений свойств.</span><span class="sxs-lookup"><span data-stu-id="7c054-148">Sometimes it may be desirable for EF Core to generate side-effects when it modifies property values.</span></span> <span data-ttu-id="7c054-149">Например, при привязке данных к сущностям задание свойства может создавать уведомления для У.И.</span><span class="sxs-lookup"><span data-stu-id="7c054-149">For example, when data binding to entities, setting a property may generate notifications to the U.I.</span></span> <span data-ttu-id="7c054-150">что не происходит при непосредственном задании поля.</span><span class="sxs-lookup"><span data-stu-id="7c054-150">which do not happen when setting the field directly.</span></span> <span data-ttu-id="7c054-151">Это можно сделать, изменив параметр <xref:Microsoft.EntityFrameworkCore.PropertyAccessMode> для:</span><span class="sxs-lookup"><span data-stu-id="7c054-151">This can be achieved by changing the <xref:Microsoft.EntityFrameworkCore.PropertyAccessMode> for:</span></span>

- <span data-ttu-id="7c054-152">Все типы сущностей в модели с использованием <xref:Microsoft.EntityFrameworkCore.ModelBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="7c054-152">All entity types in the model using <xref:Microsoft.EntityFrameworkCore.ModelBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType></span></span>
- <span data-ttu-id="7c054-153">Все свойства и переходы определенного типа сущности с помощью <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder%601.UsePropertyAccessMode%2A?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="7c054-153">All properties and navigations of a specific entity type using <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder%601.UsePropertyAccessMode%2A?displayProperty=nameWithType></span></span>
- <span data-ttu-id="7c054-154">Конкретное свойство, использующее <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="7c054-154">A specific property using <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType></span></span>
- <span data-ttu-id="7c054-155">Конкретная Навигация с помощью <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.NavigationBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="7c054-155">A specific navigation using <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.NavigationBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType></span></span>

<span data-ttu-id="7c054-156">Режим доступа к свойству и вызовет `Field` `PreferField` EF Core доступа к значению свойства через его резервное поле.</span><span class="sxs-lookup"><span data-stu-id="7c054-156">Property access modes `Field` and `PreferField` will cause EF Core to access the property value through its backing field.</span></span> <span data-ttu-id="7c054-157">Аналогичным образом, и вызовет `Property` `PreferProperty` EF Core доступа к значению свойства через его метод получения и задания.</span><span class="sxs-lookup"><span data-stu-id="7c054-157">Likewise, `Property` and `PreferProperty` will cause EF Core to access the property value through its getter and setter.</span></span>

<span data-ttu-id="7c054-158">Если `Field` `Property` используются значения или, а EF Core не может получить доступ к значению через поле или метод считывания/задания свойства соответственно, EF Core выдаст исключение.</span><span class="sxs-lookup"><span data-stu-id="7c054-158">If `Field` or `Property` are used and EF Core cannot access the value through the field or property getter/setter respectively, then EF Core will throw an exception.</span></span> <span data-ttu-id="7c054-159">Это гарантирует, что EF Core всегда использует доступ к полям и свойствам, если вы считаете это.</span><span class="sxs-lookup"><span data-stu-id="7c054-159">This ensures EF Core is always using field/property access when you think it is.</span></span>

<span data-ttu-id="7c054-160">С другой стороны, `PreferField` `PreferProperty` режимы и будут возвращаться к использованию свойства или резервного поля соответственно, если невозможно использовать предпочтительный доступ.</span><span class="sxs-lookup"><span data-stu-id="7c054-160">On the other hand, the `PreferField` and `PreferProperty` modes will fall back to using the property or backing field respectively if it is not possible to use the preferred access.</span></span> <span data-ttu-id="7c054-161">`PreferField` значение по умолчанию — EF Core 3,0, начиная с.</span><span class="sxs-lookup"><span data-stu-id="7c054-161">`PreferField` is the default from EF Core 3.0 onwards.</span></span> <span data-ttu-id="7c054-162">Это означает, что EF Core будет использовать поля каждый раз, когда это возможно, но не завершится ошибкой, если вместо этого доступ к свойству должен осуществляться через метод получения или задания.</span><span class="sxs-lookup"><span data-stu-id="7c054-162">This means EF Core will use fields whenever it can, but will not fail if a property must be accessed through its getter or setter instead.</span></span>

<span data-ttu-id="7c054-163">`FieldDuringConstruction` и `PreferFieldDuringConstruction` настройте EF Core для использования резервных полей _только при создании экземпляров сущности_.</span><span class="sxs-lookup"><span data-stu-id="7c054-163">`FieldDuringConstruction` and `PreferFieldDuringConstruction` configure EF Core to use of backing fields _only when creating entity instances_.</span></span> <span data-ttu-id="7c054-164">Это позволяет выполнять запросы без побочных эффектов getter и Set, в то время как изменения свойств по EF Core приведут к этим побочным эффектам.</span><span class="sxs-lookup"><span data-stu-id="7c054-164">This allows queries to be executed without getter and setter side effects, while later property changes by EF Core will cause these side effects.</span></span> <span data-ttu-id="7c054-165">`PreferFieldDuringConstruction` использовался по умолчанию до EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="7c054-165">`PreferFieldDuringConstruction` was the default prior to EF Core 3.0.</span></span>

<span data-ttu-id="7c054-166">В следующей таблице приведены различные режимы доступа к свойствам.</span><span class="sxs-lookup"><span data-stu-id="7c054-166">The different property access modes are summarized in the following table:</span></span>

| <span data-ttu-id="7c054-167">пропертякцессмоде</span><span class="sxs-lookup"><span data-stu-id="7c054-167">PropertyAccessMode</span></span>              | <span data-ttu-id="7c054-168">Предпочтение</span><span class="sxs-lookup"><span data-stu-id="7c054-168">Preference</span></span> | <span data-ttu-id="7c054-169">Предпочтение созданию сущностей</span><span class="sxs-lookup"><span data-stu-id="7c054-169">Preference creating entities</span></span> | <span data-ttu-id="7c054-170">Резервирование</span><span class="sxs-lookup"><span data-stu-id="7c054-170">Fallback</span></span> | <span data-ttu-id="7c054-171">Резервное Создание сущностей</span><span class="sxs-lookup"><span data-stu-id="7c054-171">Fallback creating entities</span></span>
|:--------------------------------|------------|------------------------------|----------|---------------------------
| `Field`                         | <span data-ttu-id="7c054-172">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-172">Field</span></span>      | <span data-ttu-id="7c054-173">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-173">Field</span></span>                        | <span data-ttu-id="7c054-174">Активизирует исключение</span><span class="sxs-lookup"><span data-stu-id="7c054-174">Throws</span></span>   | <span data-ttu-id="7c054-175">Активизирует исключение</span><span class="sxs-lookup"><span data-stu-id="7c054-175">Throws</span></span>
| `Property`                      | <span data-ttu-id="7c054-176">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-176">Property</span></span>   | <span data-ttu-id="7c054-177">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-177">Property</span></span>                     | <span data-ttu-id="7c054-178">Активизирует исключение</span><span class="sxs-lookup"><span data-stu-id="7c054-178">Throws</span></span>   | <span data-ttu-id="7c054-179">Активизирует исключение</span><span class="sxs-lookup"><span data-stu-id="7c054-179">Throws</span></span>
| `PreferField`                   | <span data-ttu-id="7c054-180">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-180">Field</span></span>      | <span data-ttu-id="7c054-181">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-181">Field</span></span>                        | <span data-ttu-id="7c054-182">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-182">Property</span></span> | <span data-ttu-id="7c054-183">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-183">Property</span></span>
| `PreferProperty`                | <span data-ttu-id="7c054-184">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-184">Property</span></span>   | <span data-ttu-id="7c054-185">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-185">Property</span></span>                     | <span data-ttu-id="7c054-186">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-186">Field</span></span>    | <span data-ttu-id="7c054-187">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-187">Field</span></span>
| `FieldDuringConstruction`       | <span data-ttu-id="7c054-188">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-188">Property</span></span>   | <span data-ttu-id="7c054-189">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-189">Field</span></span>                        | <span data-ttu-id="7c054-190">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-190">Field</span></span>    | <span data-ttu-id="7c054-191">Активизирует исключение</span><span class="sxs-lookup"><span data-stu-id="7c054-191">Throws</span></span>
| `PreferFieldDuringConstruction` | <span data-ttu-id="7c054-192">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-192">Property</span></span>   | <span data-ttu-id="7c054-193">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-193">Field</span></span>                        | <span data-ttu-id="7c054-194">Поле</span><span class="sxs-lookup"><span data-stu-id="7c054-194">Field</span></span>    | <span data-ttu-id="7c054-195">Свойство</span><span class="sxs-lookup"><span data-stu-id="7c054-195">Property</span></span>

## <a name="temporary-values"></a><span data-ttu-id="7c054-196">Временные значения</span><span class="sxs-lookup"><span data-stu-id="7c054-196">Temporary values</span></span>

<span data-ttu-id="7c054-197">EF Core создает временные значения ключа при отслеживании новых сущностей, которые будут иметь реальные значения ключа, создаваемые базой данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7c054-197">EF Core creates temporary key values when tracking new entities that will have real key values generated by the database when SaveChanges is called.</span></span> <span data-ttu-id="7c054-198">Общие сведения об использовании этих временных значений см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="7c054-198">See [Change Tracking in EF Core](xref:core/change-tracking/index) for an overview of how these temporary values are used.</span></span>

### <a name="accessing-temporary-values"></a><span data-ttu-id="7c054-199">Доступ к временным значениям</span><span class="sxs-lookup"><span data-stu-id="7c054-199">Accessing temporary values</span></span>

<span data-ttu-id="7c054-200">Начиная с EF Core 3,0, временные значения хранятся в средстве записи изменений и не задаются непосредственно на экземплярах сущностей.</span><span class="sxs-lookup"><span data-stu-id="7c054-200">Starting with EF Core 3.0, temporary values are stored in the change tracker and not set onto entity instances directly.</span></span> <span data-ttu-id="7c054-201">Однако эти _временные значения предоставляются_ при использовании различных механизмов [доступа к отслеживающим сущностям](xref:core/change-tracking/entity-entries).</span><span class="sxs-lookup"><span data-stu-id="7c054-201">However, these temporary values _are_ exposed when using the various mechanisms for [Accessing Tracked Entities](xref:core/change-tracking/entity-entries).</span></span> <span data-ttu-id="7c054-202">Например, следующий код обращается к временному значению с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.CurrentValues%2A?displayProperty=nameWithType> :</span><span class="sxs-lookup"><span data-stu-id="7c054-202">For example, the following code accesses a temporary value using <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.CurrentValues%2A?displayProperty=nameWithType>:</span></span>

<!--
        using var context = new BlogsContext();

        var blog = new Blog { Name = ".NET Blog" };

        context.Add(blog);

        Console.WriteLine($"Blog.Id set on entity is {blog.Id}");
        Console.WriteLine($"Blog.Id tracked by EF is {context.Entry(blog).Property(e => e.Id).CurrentValue}");
-->
[!code-csharp[Temporary_values_1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/Samples.cs?name=Temporary_values_1)]

<span data-ttu-id="7c054-203">Выходные данные этого кода:</span><span class="sxs-lookup"><span data-stu-id="7c054-203">The output from this code is:</span></span>

```output
Blog.Id set on entity is 0
Blog.Id tracked by EF is -2147482643
```

<span data-ttu-id="7c054-204"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary%2A?displayProperty=nameWithType> может использоваться для проверки временных значений.</span><span class="sxs-lookup"><span data-stu-id="7c054-204"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary%2A?displayProperty=nameWithType> can be used to check for temporary values.</span></span>

### <a name="manipulating-temporary-values"></a><span data-ttu-id="7c054-205">Работа с временными значениями</span><span class="sxs-lookup"><span data-stu-id="7c054-205">Manipulating temporary values</span></span>

<span data-ttu-id="7c054-206">Иногда бывает полезно явно работать с временными значениями.</span><span class="sxs-lookup"><span data-stu-id="7c054-206">It is sometimes useful to explicitly work with temporary values.</span></span> <span data-ttu-id="7c054-207">Например, Коллекция новых сущностей может быть создана на веб-клиенте и затем сериализована обратно на сервер.</span><span class="sxs-lookup"><span data-stu-id="7c054-207">For example, a collection of new entities might be created on a web client and then serialized back to the server.</span></span> <span data-ttu-id="7c054-208">Значения внешнего ключа — это один из способов настройки связей между этими сущностями.</span><span class="sxs-lookup"><span data-stu-id="7c054-208">Foreign key values are one way to set up relationships between these entities.</span></span> <span data-ttu-id="7c054-209">В следующем коде этот подход используется для связывания графа новых сущностей по внешнему ключу, при этом по-прежнему разрешается создавать реальные значения ключа при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7c054-209">The following code uses this approach to associate a graph of new entities by foreign key, while still allowing real key values to be generated when SaveChanges is called.</span></span>

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

<span data-ttu-id="7c054-210">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="7c054-210">Notice that:</span></span>

- <span data-ttu-id="7c054-211">Отрицательные числа используются в качестве временных значений ключей. Это не является обязательным, но является общим соглашением для предотвращения конфликта ключей.</span><span class="sxs-lookup"><span data-stu-id="7c054-211">Negative numbers are used as temporary key values; this is not required, but is a common convention to prevent key clashes.</span></span>
- <span data-ttu-id="7c054-212">`Post.BlogId`Свойству FK присваивается то же отрицательное значение, что и ПК-PK связанного блога.</span><span class="sxs-lookup"><span data-stu-id="7c054-212">The `Post.BlogId` FK property is assigned the same negative value as the PK of the associated blog.</span></span>
- <span data-ttu-id="7c054-213">Значения ПЕРВИЧного ключа помечаются как временные, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary%2A> после того как каждая сущность будет отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="7c054-213">The PK values are marked as temporary by setting <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary%2A> after each entity is tracked.</span></span> <span data-ttu-id="7c054-214">Это необходимо, так как любое значение ключа, предоставляемое приложением, считается реальным значением ключа.</span><span class="sxs-lookup"><span data-stu-id="7c054-214">This is necessary because any key value supplied by the application is assumed to be a real key value.</span></span>

<span data-ttu-id="7c054-215">[Просмотр представления отладки средства записи изменений](xref:core/change-tracking/debug-views) перед вызовом SaveChanges показывает, что значения первичного ключа помечены как временные, а записи связаны с правильными блогами, включая исправление переходов:</span><span class="sxs-lookup"><span data-stu-id="7c054-215">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) before calling SaveChanges shows that the PK values are marked as temporary and posts are associated with the correct blogs, including fixup of navigations:</span></span>

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

<span data-ttu-id="7c054-216">После вызова <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> эти временные значения были заменены реальными значениями, создаваемыми базой данных:</span><span class="sxs-lookup"><span data-stu-id="7c054-216">After calling <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>, these temporary values have been replaced by real values generated by the database:</span></span>

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

## <a name="working-with-default-values"></a><span data-ttu-id="7c054-217">Работа со значениями по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7c054-217">Working with default values</span></span>

<span data-ttu-id="7c054-218">EF Core позволяет свойству получить значение по умолчанию из базы данных при <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> вызове метода.</span><span class="sxs-lookup"><span data-stu-id="7c054-218">EF Core allows a property to get its default value from the database when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span> <span data-ttu-id="7c054-219">Как и для созданных значений ключа, EF Core будет использовать по умолчанию только из базы данных, если значение не было задано явно.</span><span class="sxs-lookup"><span data-stu-id="7c054-219">Like with generated key values, EF Core will only use a default from the database if no value has been explicitly set.</span></span> <span data-ttu-id="7c054-220">Например, рассмотрим следующий тип сущности:</span><span class="sxs-lookup"><span data-stu-id="7c054-220">For example, consider the following entity type:</span></span>

<!--
    public class Token
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime ValidFrom { get; set; }
    }
-->
[!code-csharp[Token](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Token)]

<span data-ttu-id="7c054-221">`ValidFrom`Свойство настроено для получения значения по умолчанию из базы данных:</span><span class="sxs-lookup"><span data-stu-id="7c054-221">The `ValidFrom` property is configured to get a default value from the database:</span></span>

<!--
        modelBuilder
            .Entity<Token>()
            .Property(e => e.ValidFrom)
            .HasDefaultValueSql("CURRENT_TIMESTAMP");
-->
[!code-csharp[OnModelCreating_Token](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_Token)]

<span data-ttu-id="7c054-222">При вставке сущности этого типа EF Core позволяет базе данных формировать значение, если вместо этого не задано явное значение.</span><span class="sxs-lookup"><span data-stu-id="7c054-222">When inserting an entity of this type, EF Core will let the database generate the value unless an explicit value has been set instead.</span></span> <span data-ttu-id="7c054-223">Пример:</span><span class="sxs-lookup"><span data-stu-id="7c054-223">For example:</span></span>

<!--
            using var context = new BlogsContext();

            context.AddRange(
                new Token { Name = "A" },
                new Token { Name = "B", ValidFrom = new DateTime(1111, 11, 11, 11, 11, 11)});

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Working_with_default_values_1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_1)]

<span data-ttu-id="7c054-224">[Просмотр представления отладки "средство записи изменений](xref:core/change-tracking/debug-views) " показывает, что первый маркер был `ValidFrom` создан базой данных, тогда как второй токен использовал явно заданный параметр:</span><span class="sxs-lookup"><span data-stu-id="7c054-224">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) shows that the first token had `ValidFrom` generated by the database, while the second token used the value explicitly set:</span></span>

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
> <span data-ttu-id="7c054-225">Для использования значений по умолчанию базы данных требуется, чтобы в столбце базы данных было настроено ограничение значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7c054-225">Using database default values requires that the database column has a default value constraint configured.</span></span> <span data-ttu-id="7c054-226">Это выполняется автоматически EF Core миграции при использовании <xref:Microsoft.EntityFrameworkCore.RelationalPropertyBuilderExtensions.HasDefaultValueSql%2A> или <xref:Microsoft.EntityFrameworkCore.RelationalPropertyBuilderExtensions.HasDefaultValue%2A> .</span><span class="sxs-lookup"><span data-stu-id="7c054-226">This is done automatically by EF Core migrations when using <xref:Microsoft.EntityFrameworkCore.RelationalPropertyBuilderExtensions.HasDefaultValueSql%2A> or <xref:Microsoft.EntityFrameworkCore.RelationalPropertyBuilderExtensions.HasDefaultValue%2A>.</span></span> <span data-ttu-id="7c054-227">Обязательно создайте ограничение по умолчанию для столбца каким бы то ни было другим способом, если не использовать EF Core миграции.</span><span class="sxs-lookup"><span data-stu-id="7c054-227">Make sure to create the default constraint on the column in some other way when not using EF Core migrations.</span></span>

### <a name="using-nullable-properties"></a><span data-ttu-id="7c054-228">Использование свойств, допускающих значение null</span><span class="sxs-lookup"><span data-stu-id="7c054-228">Using nullable properties</span></span>

<span data-ttu-id="7c054-229">EF Core может определить, установлено ли свойство, сравнив значение свойства со значением по умолчанию CLR для этого типа.</span><span class="sxs-lookup"><span data-stu-id="7c054-229">EF Core is able to determine whether or not a property has been set by comparing the property value to the CLR default for the that type.</span></span> <span data-ttu-id="7c054-230">Это хорошо работает в большинстве случаев, но это означает, что значение CLR по умолчанию не может быть явно вставлено в базу данных.</span><span class="sxs-lookup"><span data-stu-id="7c054-230">This works well in most cases, but means that the CLR default cannot be explicitly inserted into the database.</span></span> <span data-ttu-id="7c054-231">Например, рассмотрим сущность со свойством Integer:</span><span class="sxs-lookup"><span data-stu-id="7c054-231">For example, consider an entity with an integer property:</span></span>

<!--
public class Foo1
{
    public int Id { get; set; }
    public int Count { get; set; }
}
-->
[!code-csharp[Foo1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Foo1)]

<span data-ttu-id="7c054-232">Где это свойство настроено для использования базы данных по умолчанию, равной-1:</span><span class="sxs-lookup"><span data-stu-id="7c054-232">Where that property is configured to have a database default of -1:</span></span>

<!--
        modelBuilder
            .Entity<Foo1>()
            .Property(e => e.Count)
            .HasDefaultValue(-1);
-->
[!code-csharp[OnModelCreating_Foo1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_Foo1)]

<span data-ttu-id="7c054-233">Целью является то, что по умолчанию используется значение-1, если не задано явное.</span><span class="sxs-lookup"><span data-stu-id="7c054-233">The intention is that the default of -1 will be used whenever an explicit value is not set.</span></span> <span data-ttu-id="7c054-234">Однако установка значения 0 (значение по умолчанию CLR для целых чисел) не различается для EF Core не устанавливая никакого значения, это означает, что невозможно вставить 0 для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="7c054-234">However, setting the value to 0 (the CLR default for integers) is indistinguishable to EF Core from not setting any value, this means that it is not possible to insert 0 for this property.</span></span> <span data-ttu-id="7c054-235">Пример:</span><span class="sxs-lookup"><span data-stu-id="7c054-235">For example:</span></span>

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

<span data-ttu-id="7c054-236">Обратите внимание, что экземпляр, `Count` для которого было явно задано значение 0, по-прежнему получает из базы данных значения по умолчанию, а это не то, что мы предполагали.</span><span class="sxs-lookup"><span data-stu-id="7c054-236">Notice that the instance where `Count` was explicitly set to 0 is still gets the default value from the database, which is not what we intended.</span></span> <span data-ttu-id="7c054-237">Простой способ решения этой проблемы — сделать `Count` свойство допускающим значение NULL:</span><span class="sxs-lookup"><span data-stu-id="7c054-237">An easy way to deal with this is to make the `Count` property nullable:</span></span>

<!--
public class Foo2
{
    public int Id { get; set; }
    public int? Count { get; set; }
}
-->
[!code-csharp[Foo2](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Foo2)]

<span data-ttu-id="7c054-238">Это делает значение CLR по умолчанию NULL, а не 0, что означает, что 0 будет вставлен при явном задании:</span><span class="sxs-lookup"><span data-stu-id="7c054-238">This makes the CLR default null, instead of 0, which means 0 will now be inserted when explicitly set:</span></span>

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

### <a name="using-nullable-backing-fields"></a><span data-ttu-id="7c054-239">Использование резервных полей, допускающих значения NULL</span><span class="sxs-lookup"><span data-stu-id="7c054-239">Using nullable backing fields</span></span>

> [!NOTE]
> <span data-ttu-id="7c054-240">Шаблон резервного поля, допускающего значение null, поддерживается EF Core 5,0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="7c054-240">This nullable backing field pattern is supported by EF Core 5.0 and later.</span></span>

<span data-ttu-id="7c054-241">Проблема в том, что свойство допускает значение null, которое не может быть концептуально реализовано в модели предметной области.</span><span class="sxs-lookup"><span data-stu-id="7c054-241">The problem with making the property nullable that it may not be conceptually nullable in the domain model.</span></span> <span data-ttu-id="7c054-242">Принудительное применение значения NULL для свойства может привести к нарушению модели.</span><span class="sxs-lookup"><span data-stu-id="7c054-242">Forcing the property to be nullable therefore compromises the model.</span></span>

<span data-ttu-id="7c054-243">Начиная с EF Core 5,0, свойство может не допускать значения NULL, а только резервное поле, допускающее значение null.</span><span class="sxs-lookup"><span data-stu-id="7c054-243">Starting with EF Core 5.0, the property can be left non-nullable, with only the backing field being nullable.</span></span> <span data-ttu-id="7c054-244">Пример:</span><span class="sxs-lookup"><span data-stu-id="7c054-244">For example:</span></span>

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

<span data-ttu-id="7c054-245">Это позволяет вставить значение CLR по умолчанию (0), если свойство явно имеет значение 0, а не требуется предоставлять свойство как допускающее значение NULL в модели предметной области.</span><span class="sxs-lookup"><span data-stu-id="7c054-245">This allows the CLR default (0) to be inserted if the property is explicitly set to 0, while not needing to expose the property as nullable in the domain model.</span></span> <span data-ttu-id="7c054-246">Пример:</span><span class="sxs-lookup"><span data-stu-id="7c054-246">For example:</span></span>

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

#### <a name="nullable-backing-fields-for-bool-properties"></a><span data-ttu-id="7c054-247">Резервные поля, допускающие значения NULL для свойств bool</span><span class="sxs-lookup"><span data-stu-id="7c054-247">Nullable backing fields for bool properties</span></span>

<span data-ttu-id="7c054-248">Этот шаблон особенно полезен при использовании свойств bool с создаваемыми по умолчанию хранилищами.</span><span class="sxs-lookup"><span data-stu-id="7c054-248">This pattern is especially useful when using bool properties with store-generated defaults.</span></span> <span data-ttu-id="7c054-249">Поскольку значение CLR по умолчанию для `bool` равно "false", это означает, что "false" не может быть явно вставлено с использованием обычного шаблона.</span><span class="sxs-lookup"><span data-stu-id="7c054-249">Since the CLR default for `bool` is "false", it means that "false" cannot be inserted explicitly using the normal pattern.</span></span> <span data-ttu-id="7c054-250">Например, рассмотрим `User` тип сущности:</span><span class="sxs-lookup"><span data-stu-id="7c054-250">For example, consider a `User` entity type:</span></span>

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

<span data-ttu-id="7c054-251">Для `IsAuthorized` свойства задано значение по умолчанию "true" для базы данных:</span><span class="sxs-lookup"><span data-stu-id="7c054-251">The `IsAuthorized` property is configured with a database default value of "true":</span></span>

<!--
        modelBuilder
            .Entity<User>()
            .Property(e => e.IsAuthorized)
            .HasDefaultValue(true);
-->
[!code-csharp[OnModelCreating_User](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_User)]

<span data-ttu-id="7c054-252">`IsAuthorized`Для свойства можно задать значение "true" или "false" явным образом перед вставкой, или же можно оставить параметр неопределенным, если будет использоваться база данных по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="7c054-252">The `IsAuthorized` property can be set to "true" or "false" explicitly before inserting, or can be left unset in which case the database default will be used:</span></span>

<!--
        using var context = new BlogsContext();

        var userA = new User { Name = "Mac" };
        var userB = new User { Name = "Alice", IsAuthorized = true };
        var userC = new User { Name = "Baxter", IsAuthorized = false }; // Always deny Baxter access!

        context.AddRange(userA, userB, userC);

        context.SaveChanges();
-->
[!code-csharp[Working_with_default_values_5](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_5)]

<span data-ttu-id="7c054-253">Выходные данные SaveChanges при использовании SQLite показывают, что по умолчанию используется база данных для Mac, а для Алисы и Бакстер заданы явные значения:</span><span class="sxs-lookup"><span data-stu-id="7c054-253">The output from SaveChanges when using SQLite shows that the database default is used for Mac, while explicit values are set for Alice and Baxter:</span></span>

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

### <a name="schema-defaults-only"></a><span data-ttu-id="7c054-254">Только схемы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7c054-254">Schema defaults only</span></span>

<span data-ttu-id="7c054-255">Иногда бывает полезно иметь значения по умолчанию в схеме базы данных, созданной EF Core миграции, без EF Core использования этих значений для вставок.</span><span class="sxs-lookup"><span data-stu-id="7c054-255">Sometimes it is useful to have defaults in the database schema created by EF Core migrations without EF Core ever using these values for inserts.</span></span> <span data-ttu-id="7c054-256">Это можно сделать, настроив свойство так, как <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.ValueGeneratedNever%2A?displayProperty=nameWithType> показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7c054-256">This can be achieved by configuring the property as <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.ValueGeneratedNever%2A?displayProperty=nameWithType> For example:</span></span>

<!--
        modelBuilder
            .Entity<Bar>()
            .Property(e => e.Count)
            .HasDefaultValue(-1)
            .ValueGeneratedNever();
-->
[!code-csharp[OnModelCreating_Bar](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_Bar)]
