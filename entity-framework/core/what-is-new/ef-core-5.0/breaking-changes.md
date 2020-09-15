---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 09/08/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: bc6db48edcd7406b31ec2b4369cabf5d55fb4578
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618675"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="e5ba5-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="e5ba5-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="e5ba5-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="e5ba5-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="e5ba5-105">Summary</span></span>

| <span data-ttu-id="e5ba5-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="e5ba5-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="e5ba5-108">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="e5ba5-108">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="e5ba5-109">Средний</span><span class="sxs-lookup"><span data-stu-id="e5ba5-109">Medium</span></span>     |
| [<span data-ttu-id="e5ba5-110">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-110">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="e5ba5-111">Низкий</span><span class="sxs-lookup"><span data-stu-id="e5ba5-111">Low</span></span>        |
| [<span data-ttu-id="e5ba5-112">Генераторы значений вызываются, когда состояние сущности меняется с «отсоединено» на «не изменено», «обновлено» или «удалено»</span><span class="sxs-lookup"><span data-stu-id="e5ba5-112">Value generators are called when the entity state is changed from Detached to Unchanged, Updated or Deleted</span></span>](#non-added-generation)  | <span data-ttu-id="e5ba5-113">Низкий</span><span class="sxs-lookup"><span data-stu-id="e5ba5-113">Low</span></span>        |
| [<span data-ttu-id="e5ba5-114">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="e5ba5-114">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="e5ba5-115">Низкий</span><span class="sxs-lookup"><span data-stu-id="e5ba5-115">Low</span></span>        |
| [<span data-ttu-id="e5ba5-116">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="e5ba5-116">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="e5ba5-117">Низкий</span><span class="sxs-lookup"><span data-stu-id="e5ba5-117">Low</span></span>        |

<a name="geometric-sqlite"></a>
### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="e5ba5-118">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-118">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="e5ba5-119">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="e5ba5-119">Tracking Issue #14257</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

<span data-ttu-id="e5ba5-120">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-120">**Old behavior**</span></span>

<span data-ttu-id="e5ba5-121">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-121">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="e5ba5-122">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-122">However, it only ever affected database creation.</span></span> <span data-ttu-id="e5ba5-123">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-123">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="e5ba5-124">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="e5ba5-124">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span></span>

<span data-ttu-id="e5ba5-125">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-125">**New behavior**</span></span>

<span data-ttu-id="e5ba5-126">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-126">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="e5ba5-127">Это более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-127">This more closely matches the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

<span data-ttu-id="e5ba5-128">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-128">**Why**</span></span>

<span data-ttu-id="e5ba5-129">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-129">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

<span data-ttu-id="e5ba5-130">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-130">**Mitigations**</span></span>

<span data-ttu-id="e5ba5-131">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="e5ba5-131">Use `HasColumnType` to specify the dimension:</span></span>

```cs
modelBuilder.Entity<GeoEntity>(
    x =>
    {
        // Allow any GEOMETRY value with optional Z and M values
        x.Property(e => e.Geometry).HasColumnType("GEOMETRYZM");

        // Allow only POINT values with an optional Z value
        x.Property(e => e.Point).HasColumnType("POINTZ");
    });
```

<a name="required-dependent"></a>
### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="e5ba5-132">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="e5ba5-132">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="e5ba5-133">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="e5ba5-133">Tracking Issue #17286</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17286)

<span data-ttu-id="e5ba5-134">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-134">**Old behavior**</span></span>

<span data-ttu-id="e5ba5-135">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-135">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="e5ba5-136">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-136">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

<span data-ttu-id="e5ba5-137">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-137">**New behavior**</span></span>

<span data-ttu-id="e5ba5-138">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения, и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-138">With the added support for required dependents it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="e5ba5-139">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-139">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

<span data-ttu-id="e5ba5-140">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-140">**Why**</span></span>

<span data-ttu-id="e5ba5-141">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="e5ba5-141">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

<span data-ttu-id="e5ba5-142">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-142">**Mitigations**</span></span>

<span data-ttu-id="e5ba5-143">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-143">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="non-added-generation"></a>
### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="e5ba5-144">Генераторы значений вызываются, когда состояние сущности меняется с «отсоединено» на «не изменено», «обновлено» или «удалено»</span><span class="sxs-lookup"><span data-stu-id="e5ba5-144">Value generators are called when the entity state is changed from Detached to Unchanged, Updated or Deleted</span></span>

[<span data-ttu-id="e5ba5-145">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="e5ba5-145">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="e5ba5-146">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-146">**Old behavior**</span></span>

<span data-ttu-id="e5ba5-147">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="e5ba5-147">Value generators were only called when the entity state changed to Added.</span></span>

<span data-ttu-id="e5ba5-148">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-148">**New behavior**</span></span>

<span data-ttu-id="e5ba5-149">Генераторы значений теперь вызываются, когда состояние сущности меняется с «отсоединено» на «не изменено», «обновлено» или «удалено», а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-149">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated or Deleted and the property contains the default values.</span></span>

<span data-ttu-id="e5ba5-150">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-150">**Why**</span></span>

<span data-ttu-id="e5ba5-151">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-151">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

<span data-ttu-id="e5ba5-152">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-152">**Mitigations**</span></span>

<span data-ttu-id="e5ba5-153">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-153">To prevent the value generator from being called assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>
### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="e5ba5-154">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="e5ba5-154">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="e5ba5-155">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="e5ba5-155">Tracking Issue #20305</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/20305)

<span data-ttu-id="e5ba5-156">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-156">**Old behavior**</span></span>

<span data-ttu-id="e5ba5-157">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-157">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

<span data-ttu-id="e5ba5-158">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-158">**New behavior**</span></span>

<span data-ttu-id="e5ba5-159">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-159">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="e5ba5-160">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-160">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

<span data-ttu-id="e5ba5-161">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-161">**Why**</span></span>

<span data-ttu-id="e5ba5-162">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-162">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="e5ba5-163">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-163">Using it to find differences is faster and more accurate.</span></span>

<span data-ttu-id="e5ba5-164">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-164">**Mitigations**</span></span>

<span data-ttu-id="e5ba5-165">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-165">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

```cs
var dependencies = context.GetService<ProviderConventionSetBuilderDependencies>();
var relationalDependencies = context.GetService<RelationalConventionSetBuilderDependencies>();

var typeMappingConvention = new TypeMappingConvention(dependencies);
typeMappingConvention.ProcessModelFinalizing(((IConventionModel)modelSnapshot.Model).Builder, null);

var relationalModelConvention = new RelationalModelConvention(dependencies, relationalDependencies);
var sourceModel = relationalModelConvention.ProcessModelFinalized(snapshot.Model);

var modelDiffer = context.GetService<IMigrationsModelDiffer>();
var hasDifferences = modelDiffer.HasDifferences(
    ((IMutableModel)sourceModel).FinalizeModel().GetRelationalModel(),
    context.Model.GetRelationalModel());
```

<span data-ttu-id="e5ba5-166">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="e5ba5-166">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="read-only-discriminators"></a>
### <a name="discriminators-are-read-only"></a><span data-ttu-id="e5ba5-167">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="e5ba5-167">Discriminators are read-only</span></span>

[<span data-ttu-id="e5ba5-168">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="e5ba5-168">Tracking Issue #21154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/21154)

<span data-ttu-id="e5ba5-169">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-169">**Old behavior**</span></span>

<span data-ttu-id="e5ba5-170">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="e5ba5-170">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

<span data-ttu-id="e5ba5-171">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-171">**New behavior**</span></span>

<span data-ttu-id="e5ba5-172">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-172">An exception will be throws in the above case.</span></span>

<span data-ttu-id="e5ba5-173">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-173">**Why**</span></span>

<span data-ttu-id="e5ba5-174">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-174">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state which might result in unexpected behavior.</span></span>

<span data-ttu-id="e5ba5-175">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e5ba5-175">**Mitigations**</span></span>

<span data-ttu-id="e5ba5-176">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="e5ba5-176">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges` the discriminator can be made mutable:</span></span>

```cs
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```
