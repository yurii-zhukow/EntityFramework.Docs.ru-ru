---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 09/24/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: e64f2b387d236e96d0451f3d55b3241daaba32d8
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065645"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="b04d1-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="b04d1-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="b04d1-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="b04d1-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="b04d1-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="b04d1-105">Summary</span></span>

| <span data-ttu-id="b04d1-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="b04d1-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="b04d1-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="b04d1-108">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="b04d1-108">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="b04d1-109">Средний</span><span class="sxs-lookup"><span data-stu-id="b04d1-109">Medium</span></span>     |
| [<span data-ttu-id="b04d1-110">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="b04d1-110">Defining query is replaced with provider-specific methods</span></span>](#defining-query)                                                          | <span data-ttu-id="b04d1-111">Средний</span><span class="sxs-lookup"><span data-stu-id="b04d1-111">Medium</span></span>     |
| [<span data-ttu-id="b04d1-112">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="b04d1-112">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="b04d1-113">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-113">Low</span></span>        |
| [<span data-ttu-id="b04d1-114">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="b04d1-114">Cosmos: Partition key is now added to the primary key</span></span>](#cosmos-partition-key)                                                        | <span data-ttu-id="b04d1-115">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-115">Low</span></span>        |
| [<span data-ttu-id="b04d1-116">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="b04d1-116">Cosmos: `id` property renamed to `__id`</span></span>](#cosmos-id)                                                                                 | <span data-ttu-id="b04d1-117">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-117">Low</span></span>        |
| <span data-ttu-id="b04d1-118">[Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)</span><span class="sxs-lookup"><span data-stu-id="b04d1-118">[Cosmos: byte[] is now stored as a base64 string instead of a number array](#cosmos-byte)</span></span>                                             | <span data-ttu-id="b04d1-119">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-119">Low</span></span>        |
| [<span data-ttu-id="b04d1-120">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="b04d1-120">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>](#cosmos-metadata)                                                          | <span data-ttu-id="b04d1-121">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-121">Low</span></span>        |
| [<span data-ttu-id="b04d1-122">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="b04d1-122">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>](#non-added-generation) | <span data-ttu-id="b04d1-123">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-123">Low</span></span>        |
| [<span data-ttu-id="b04d1-124">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="b04d1-124">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="b04d1-125">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-125">Low</span></span>        |
| [<span data-ttu-id="b04d1-126">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="b04d1-126">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="b04d1-127">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-127">Low</span></span>        |
| [<span data-ttu-id="b04d1-128">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="b04d1-128">Provider-specific EF.Functions methods throw for InMemory provider</span></span>](#no-client-methods)                                              | <span data-ttu-id="b04d1-129">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-129">Low</span></span>        |
| [<span data-ttu-id="b04d1-130">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="b04d1-130">IndexBuilder.HasName is now obsolete</span></span>](#index-obsolete)                                                                               | <span data-ttu-id="b04d1-131">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-131">Low</span></span>        |
| [<span data-ttu-id="b04d1-132">Добавлено средство для преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="b04d1-132">A pluarlizer is now included for scaffolding reverse engineered models</span></span>](#pluralizer)                                                 | <span data-ttu-id="b04d1-133">Низкий</span><span class="sxs-lookup"><span data-stu-id="b04d1-133">Low</span></span>        |

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="b04d1-134">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="b04d1-134">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="b04d1-135">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="b04d1-135">Tracking Issue #14257</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

<span data-ttu-id="b04d1-136">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-136">**Old behavior**</span></span>

<span data-ttu-id="b04d1-137">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="b04d1-137">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="b04d1-138">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="b04d1-138">However, it only ever affected database creation.</span></span> <span data-ttu-id="b04d1-139">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="b04d1-139">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="b04d1-140">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="b04d1-140">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span></span>

<span data-ttu-id="b04d1-141">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-141">**New behavior**</span></span>

<span data-ttu-id="b04d1-142">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="b04d1-142">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="b04d1-143">Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="b04d1-143">This API matches more closely to the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

<span data-ttu-id="b04d1-144">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-144">**Why**</span></span>

<span data-ttu-id="b04d1-145">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="b04d1-145">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

<span data-ttu-id="b04d1-146">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-146">**Mitigations**</span></span>

<span data-ttu-id="b04d1-147">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="b04d1-147">Use `HasColumnType` to specify the dimension:</span></span>

```csharp
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

### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="b04d1-148">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="b04d1-148">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="b04d1-149">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="b04d1-149">Tracking Issue #17286</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17286)

<span data-ttu-id="b04d1-150">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-150">**Old behavior**</span></span>

<span data-ttu-id="b04d1-151">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="b04d1-151">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="b04d1-152">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="b04d1-152">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

<span data-ttu-id="b04d1-153">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-153">**New behavior**</span></span>

<span data-ttu-id="b04d1-154">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="b04d1-154">With the added support for required dependents, it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="b04d1-155">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="b04d1-155">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

<span data-ttu-id="b04d1-156">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-156">**Why**</span></span>

<span data-ttu-id="b04d1-157">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="b04d1-157">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

<span data-ttu-id="b04d1-158">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-158">**Mitigations**</span></span>

<span data-ttu-id="b04d1-159">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-159">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="cosmos-partition-key"></a>

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a><span data-ttu-id="b04d1-160">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="b04d1-160">Cosmos: Partition key is now added to the primary key</span></span>

[<span data-ttu-id="b04d1-161">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="b04d1-161">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="b04d1-162">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-162">**Old behavior**</span></span>

<span data-ttu-id="b04d1-163">Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-163">The partition key property was only added to the alternate key that includes `id`.</span></span>

<span data-ttu-id="b04d1-164">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-164">**New behavior**</span></span>

<span data-ttu-id="b04d1-165">В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="b04d1-165">The partition key property is now also added to the primary key by convention.</span></span>

<span data-ttu-id="b04d1-166">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-166">**Why**</span></span>

<span data-ttu-id="b04d1-167">Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="b04d1-167">This change makes the model better aligned with Azure Cosmos DB semantics and improves the performance of `Find` and some queries.</span></span>

<span data-ttu-id="b04d1-168">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-168">**Mitigations**</span></span>

<span data-ttu-id="b04d1-169">Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-169">To prevent the partition key property to be added to the primary key, configure it in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a><span data-ttu-id="b04d1-170">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="b04d1-170">Cosmos: `id` property renamed to `__id`</span></span>

[<span data-ttu-id="b04d1-171">Отслеживание вопроса № 17751</span><span class="sxs-lookup"><span data-stu-id="b04d1-171">Tracking Issue #17751</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17751)

<span data-ttu-id="b04d1-172">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-172">**Old behavior**</span></span>

<span data-ttu-id="b04d1-173">Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-173">The shadow property mapped to the `id` JSON property was also named `id`.</span></span>

<span data-ttu-id="b04d1-174">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-174">**New behavior**</span></span>

<span data-ttu-id="b04d1-175">Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-175">The shadow property created by convention is now named `__id`.</span></span>

<span data-ttu-id="b04d1-176">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-176">**Why**</span></span>

<span data-ttu-id="b04d1-177">Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.</span><span class="sxs-lookup"><span data-stu-id="b04d1-177">This change makes it less likely that the `id` property clashes with an existing property on the entity type.</span></span>

<span data-ttu-id="b04d1-178">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-178">**Mitigations**</span></span>

<span data-ttu-id="b04d1-179">Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-179">To go back to the 3.x behavior, configure the `id` property in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a><span data-ttu-id="b04d1-180">Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива</span><span class="sxs-lookup"><span data-stu-id="b04d1-180">Cosmos: byte[] is now stored as a base64 string instead of a number array</span></span>

[<span data-ttu-id="b04d1-181">Отслеживание вопроса № 17306</span><span class="sxs-lookup"><span data-stu-id="b04d1-181">Tracking Issue #17306</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17306)

<span data-ttu-id="b04d1-182">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-182">**Old behavior**</span></span>

<span data-ttu-id="b04d1-183">Свойства типа byte[] хранились в виде числового массива.</span><span class="sxs-lookup"><span data-stu-id="b04d1-183">Properties of type byte[] were stored as a number array.</span></span>

<span data-ttu-id="b04d1-184">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-184">**New behavior**</span></span>

<span data-ttu-id="b04d1-185">Свойства типа byte[] теперь хранятся в виде строки base64.</span><span class="sxs-lookup"><span data-stu-id="b04d1-185">Properties of type byte[] are now stored as a base64 string.</span></span>

<span data-ttu-id="b04d1-186">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-186">**Why**</span></span>

<span data-ttu-id="b04d1-187">Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.</span><span class="sxs-lookup"><span data-stu-id="b04d1-187">This representation of byte[] aligns better with expectations and is the default behavior of the major JSON serialization libraries.</span></span>

<span data-ttu-id="b04d1-188">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-188">**Mitigations**</span></span>

<span data-ttu-id="b04d1-189">Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки.</span><span class="sxs-lookup"><span data-stu-id="b04d1-189">Existing data stored as number arrays will still be queried correctly, but currently there isn't a supported way to change back the insert behavior.</span></span> <span data-ttu-id="b04d1-190">Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/aspnet/EntityFrameworkCore/issues/17306)</span><span class="sxs-lookup"><span data-stu-id="b04d1-190">If this limitation is blocking your scenario, comment on [this issue](https://github.com/aspnet/EntityFrameworkCore/issues/17306)</span></span>

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a><span data-ttu-id="b04d1-191">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="b04d1-191">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>

[<span data-ttu-id="b04d1-192">Отслеживание вопроса № 17874</span><span class="sxs-lookup"><span data-stu-id="b04d1-192">Tracking Issue #17874</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17874)

<span data-ttu-id="b04d1-193">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-193">**Old behavior**</span></span>

<span data-ttu-id="b04d1-194">Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-194">Previously the extension methods were called `GetPropertyName` and `SetPropertyName`</span></span>

<span data-ttu-id="b04d1-195">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-195">**New behavior**</span></span>

<span data-ttu-id="b04d1-196">Старый интерфейс API был удален, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-196">The old API was removed and new methods added: `GetJsonPropertyName`, `SetJsonPropertyName`</span></span>

<span data-ttu-id="b04d1-197">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-197">**Why**</span></span>

<span data-ttu-id="b04d1-198">Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.</span><span class="sxs-lookup"><span data-stu-id="b04d1-198">This change removes the ambiguity around what these methods are configuring.</span></span>

<span data-ttu-id="b04d1-199">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-199">**Mitigations**</span></span>

<span data-ttu-id="b04d1-200">Используйте новый API.</span><span class="sxs-lookup"><span data-stu-id="b04d1-200">Use the new API.</span></span>

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="b04d1-201">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="b04d1-201">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>

[<span data-ttu-id="b04d1-202">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="b04d1-202">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="b04d1-203">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-203">**Old behavior**</span></span>

<span data-ttu-id="b04d1-204">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="b04d1-204">Value generators were only called when the entity state changed to Added.</span></span>

<span data-ttu-id="b04d1-205">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-205">**New behavior**</span></span>

<span data-ttu-id="b04d1-206">Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b04d1-206">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated, or Deleted and the property contains the default values.</span></span>

<span data-ttu-id="b04d1-207">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-207">**Why**</span></span>

<span data-ttu-id="b04d1-208">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="b04d1-208">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

<span data-ttu-id="b04d1-209">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-209">**Mitigations**</span></span>

<span data-ttu-id="b04d1-210">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="b04d1-210">To prevent the value generator from being called, assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>

### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="b04d1-211">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="b04d1-211">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="b04d1-212">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="b04d1-212">Tracking Issue #20305</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/20305)

<span data-ttu-id="b04d1-213">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-213">**Old behavior**</span></span>

<span data-ttu-id="b04d1-214">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-214">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

<span data-ttu-id="b04d1-215">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-215">**New behavior**</span></span>

<span data-ttu-id="b04d1-216">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-216">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="b04d1-217">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="b04d1-217">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

<span data-ttu-id="b04d1-218">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-218">**Why**</span></span>

<span data-ttu-id="b04d1-219">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="b04d1-219">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="b04d1-220">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="b04d1-220">Using it to find differences is faster and more accurate.</span></span>

<span data-ttu-id="b04d1-221">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-221">**Mitigations**</span></span>

<span data-ttu-id="b04d1-222">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-222">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

```csharp
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

<span data-ttu-id="b04d1-223">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="b04d1-223">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="read-only-discriminators"></a>

### <a name="discriminators-are-read-only"></a><span data-ttu-id="b04d1-224">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="b04d1-224">Discriminators are read-only</span></span>

[<span data-ttu-id="b04d1-225">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="b04d1-225">Tracking Issue #21154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/21154)

<span data-ttu-id="b04d1-226">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-226">**Old behavior**</span></span>

<span data-ttu-id="b04d1-227">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="b04d1-227">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

<span data-ttu-id="b04d1-228">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-228">**New behavior**</span></span>

<span data-ttu-id="b04d1-229">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="b04d1-229">An exception will be throws in the above case.</span></span>

<span data-ttu-id="b04d1-230">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-230">**Why**</span></span>

<span data-ttu-id="b04d1-231">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="b04d1-231">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state, which might result in unexpected behavior.</span></span>

<span data-ttu-id="b04d1-232">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-232">**Mitigations**</span></span>

<span data-ttu-id="b04d1-233">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="b04d1-233">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges`, the discriminator can be made mutable:</span></span>

```csharp
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a><span data-ttu-id="b04d1-234">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="b04d1-234">Defining query is replaced with provider-specific methods</span></span>

[<span data-ttu-id="b04d1-235">Отслеживание вопроса № 18903</span><span class="sxs-lookup"><span data-stu-id="b04d1-235">Tracking Issue #18903</span></span>](https://github.com/dotnet/efcore/issues/18903)

<span data-ttu-id="b04d1-236">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-236">**Old behavior**</span></span>

<span data-ttu-id="b04d1-237">Типы сущностей сопоставлялись с определяющими запросами на уровне ядра.</span><span class="sxs-lookup"><span data-stu-id="b04d1-237">Entity types were mapped to defining queries at the Core level.</span></span> <span data-ttu-id="b04d1-238">Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.</span><span class="sxs-lookup"><span data-stu-id="b04d1-238">Anytime the entity type was used in the query root of the entity type was replaced by the defining query for any provider.</span></span>

<span data-ttu-id="b04d1-239">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-239">**New behavior**</span></span>

<span data-ttu-id="b04d1-240">Интерфейсы API для определяющего запроса стали нерекомендуемыми.</span><span class="sxs-lookup"><span data-stu-id="b04d1-240">APIs for defining query are deprecated.</span></span> <span data-ttu-id="b04d1-241">Появились новые интерфейсы API для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="b04d1-241">New provider-specific APIs were introduced.</span></span>

<span data-ttu-id="b04d1-242">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-242">**Why**</span></span>

<span data-ttu-id="b04d1-243">Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.</span><span class="sxs-lookup"><span data-stu-id="b04d1-243">While defining queries were implemented as replacement query whenever query root is used in the query, it had a few issues:</span></span>

- <span data-ttu-id="b04d1-244">Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.</span><span class="sxs-lookup"><span data-stu-id="b04d1-244">If defining query is projecting entity type using `new { ... }` in `Select` method, then identifying that as an entity required additional work and made it inconsistent with how EF Core treats nominal types in the query.</span></span>
- <span data-ttu-id="b04d1-245">Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.</span><span class="sxs-lookup"><span data-stu-id="b04d1-245">For relational providers `FromSql` is still needed to pass the SQL string in LINQ expression form.</span></span>

<span data-ttu-id="b04d1-246">Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных).</span><span class="sxs-lookup"><span data-stu-id="b04d1-246">Initially defining queries were introduced as client-side views to be used with In-Memory provider for keyless entities (similar to database views in relational databases).</span></span> <span data-ttu-id="b04d1-247">Такое определение упрощает тестирование приложения с базой данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="b04d1-247">Such definition makes it easy to test application against in-memory database.</span></span> <span data-ttu-id="b04d1-248">Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание.</span><span class="sxs-lookup"><span data-stu-id="b04d1-248">Afterwards they became broadly applicable, which was useful but brought inconsistent and hard to understand behavior.</span></span> <span data-ttu-id="b04d1-249">Поэтому мы решили упростить принцип их работы.</span><span class="sxs-lookup"><span data-stu-id="b04d1-249">So we decided to simplify the concept.</span></span> <span data-ttu-id="b04d1-250">Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому.</span><span class="sxs-lookup"><span data-stu-id="b04d1-250">We made LINQ based defining query exclusive to In-Memory provider and treat them differently.</span></span> <span data-ttu-id="b04d1-251">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).</span><span class="sxs-lookup"><span data-stu-id="b04d1-251">For more information, [see this issue](https://github.com/dotnet/efcore/issues/20023).</span></span>

<span data-ttu-id="b04d1-252">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-252">**Mitigations**</span></span>

<span data-ttu-id="b04d1-253">Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="b04d1-253">For relational providers, use `ToSqlQuery` method in `OnModelCreating` and pass in a SQL string to use for the entity type.</span></span>
<span data-ttu-id="b04d1-254">Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="b04d1-254">For the In-Memory provider, use `ToInMemoryQuery` method in `OnModelCreating` and pass in a LINQ query to use for the entity type.</span></span>

<a name="no-client-methods"></a>

### <a name="provider-specific-effunctions-methods-throw-for-inmemory-provider"></a><span data-ttu-id="b04d1-255">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="b04d1-255">Provider-specific EF.Functions methods throw for InMemory provider</span></span>

[<span data-ttu-id="b04d1-256">Отслеживание вопроса № 20294</span><span class="sxs-lookup"><span data-stu-id="b04d1-256">Tracking Issue #20294</span></span>](https://github.com/dotnet/efcore/issues/20294)

<span data-ttu-id="b04d1-257">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-257">**Old behavior**</span></span>

<span data-ttu-id="b04d1-258">Относящиеся к конкретному поставщику методы EF.Functions содержали реализацию для выполнения на стороне клиента, что позволяло им выполняться в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="b04d1-258">Provider-specific EF.Functions methods contained implementation for client execution, which allowed them to be executed on the InMemory provider.</span></span> <span data-ttu-id="b04d1-259">Например, `EF.Functions.DateDiffDay` — это относящийся к SQL Server метод, который работал в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="b04d1-259">For example, `EF.Functions.DateDiffDay` is a Sql Server specific method, which worked on InMemory provider.</span></span>

<span data-ttu-id="b04d1-260">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-260">**New behavior**</span></span>

<span data-ttu-id="b04d1-261">Методы, относящиеся к конкретному поставщику, были изменены так, чтобы в теле метода создавалось исключение, блокирующее вычисление метода на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b04d1-261">Provider-specific methods have been updated to throw an exception in their method body to block evaluating them on client side.</span></span>

<span data-ttu-id="b04d1-262">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-262">**Why**</span></span>

<span data-ttu-id="b04d1-263">Методы, относящиеся к конкретному поставщику, сопоставляются с функцией базы данных.</span><span class="sxs-lookup"><span data-stu-id="b04d1-263">Provider-specific methods map to a database function.</span></span> <span data-ttu-id="b04d1-264">Вычисление, выполняемое сопоставленной функцией базы данных, не всегда может реплицироваться на стороне клиента в LINQ.</span><span class="sxs-lookup"><span data-stu-id="b04d1-264">The computation done by the mapped database function can't always be replicated on the client side in LINQ.</span></span> <span data-ttu-id="b04d1-265">Поэтому результаты, полученные при выполнении одного и того же метода на сервере и в клиенте, могут различаться.</span><span class="sxs-lookup"><span data-stu-id="b04d1-265">It may cause the result from the server to differ when executing the same method on client.</span></span> <span data-ttu-id="b04d1-266">Так как эти методы используются в LINQ для преобразования в определенные функции базы данных, их не нужно вычислять на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b04d1-266">Since these methods are used in LINQ to translate to specific database functions, they don't need to be evaluated on client side.</span></span> <span data-ttu-id="b04d1-267">Так как поставщик InMemory — это другая *база данных*, эти методы недоступны для него.</span><span class="sxs-lookup"><span data-stu-id="b04d1-267">As InMemory provider is a different *database*, these methods aren't available for this provider.</span></span> <span data-ttu-id="b04d1-268">При попытке выполнить их для поставщика InMemory или любого другого поставщика, который не преобразует эти методы, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="b04d1-268">Trying to execute them for InMemory provider, or any other provider that doesn't translate these methods, throws an exception.</span></span>

<span data-ttu-id="b04d1-269">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-269">**Mitigations**</span></span>

<span data-ttu-id="b04d1-270">Так как точно воссоздать поведение функций базы данных невозможно, содержащие их запросы следует тестировать на основе базы данных того же типа, что и рабочая база данных.</span><span class="sxs-lookup"><span data-stu-id="b04d1-270">Since there's no way to mimic behavior of database functions accurately, you should test the queries containing them against same kind of database as in production.</span></span>

<a name="index-obsolete"></a>

### <a name="indexbuilderhasname-is-now-obsolete"></a><span data-ttu-id="b04d1-271">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="b04d1-271">IndexBuilder.HasName is now obsolete</span></span>

[<span data-ttu-id="b04d1-272">Отслеживание проблемы № 21089</span><span class="sxs-lookup"><span data-stu-id="b04d1-272">Tracking Issue #21089</span></span>](https://github.com/dotnet/efcore/issues/21089)

<span data-ttu-id="b04d1-273">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-273">**Old behavior**</span></span>

<span data-ttu-id="b04d1-274">Ранее допускалось определение только одного индекса для конкретного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="b04d1-274">Previously, only one index could be defined over a given set of properties.</span></span> <span data-ttu-id="b04d1-275">Имя для базы данных индекса настраивалось с использованием IndexBuilder.HasName.</span><span class="sxs-lookup"><span data-stu-id="b04d1-275">The database name of an index was configured using IndexBuilder.HasName.</span></span>

<span data-ttu-id="b04d1-276">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-276">**New behavior**</span></span>

<span data-ttu-id="b04d1-277">Теперь допускается несколько индексов для одного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="b04d1-277">Multiple indexes are now allowed on the same set or properties.</span></span> <span data-ttu-id="b04d1-278">Эти индексы отличаются именами в пределах модели.</span><span class="sxs-lookup"><span data-stu-id="b04d1-278">These indexes are now distinguished by a name in the model.</span></span> <span data-ttu-id="b04d1-279">По соглашению имя модели используется в качестве имени базы данных, но его также можно указать независимо с использованием HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="b04d1-279">By convention, the model name is used as the database name; however it can also be configured independently using HasDatabaseName.</span></span>

<span data-ttu-id="b04d1-280">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-280">**Why**</span></span>

<span data-ttu-id="b04d1-281">В будущем мы хотим включить для набора свойств параллельные индексы с сортировкой по возрастанию и убыванию, а также индексы с разными параметрами сортировки.</span><span class="sxs-lookup"><span data-stu-id="b04d1-281">In the future, we'd like to enable both ascending and descending indexes or indexes with different collations on the same set of properties.</span></span> <span data-ttu-id="b04d1-282">Это изменение продвигает нас в этом направлении.</span><span class="sxs-lookup"><span data-stu-id="b04d1-282">This change moves us another step in that direction.</span></span>

<span data-ttu-id="b04d1-283">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-283">**Mitigations**</span></span>

<span data-ttu-id="b04d1-284">Любой код, который ранее вызывал IndexBuilder.HasName, необходимо изменить для вызова HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="b04d1-284">Any code that was previously calling IndexBuilder.HasName should be updated to call HasDatabaseName instead.</span></span>

<span data-ttu-id="b04d1-285">Если в проекте есть миграции, созданные до версии EF Core 2.0.0, вы можете игнорировать предупреждения для этих файлов, подавив их с помощью `#pragma warning disable 612, 618`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-285">If your project includes migrations generated prior to EF Core version 2.0.0, you can safely ignore the warning in those files and suppress it by adding `#pragma warning disable 612, 618`.</span></span>

<a name="pluralizer"></a>

### <a name="a-pluarlizer-is-now-included-for-scaffolding-reverse-engineered-models"></a><span data-ttu-id="b04d1-286">Добавлено средство для преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="b04d1-286">A pluarlizer is now included for scaffolding reverse engineered models</span></span>

[<span data-ttu-id="b04d1-287">Отслеживание проблемы № 11160</span><span class="sxs-lookup"><span data-stu-id="b04d1-287">Tracking Issue #11160</span></span>](https://github.com/dotnet/efcore/issues/11160)

<span data-ttu-id="b04d1-288">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-288">**Old behavior**</span></span>

<span data-ttu-id="b04d1-289">Ранее нужно было устанавливать отдельный пакет, чтобы преобразовывать имена DbSet и коллекций во множественное число и имена таблиц в единственное число при создании шаблонов DbContext и типов сущностей путем реконструирования из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="b04d1-289">Previously, you had to install a separate pluralizer package in order to pluralize DbSet and collection navigation names and singularize table names when scaffoding a DbContext and entity types by reverse engineering a database schema.</span></span>

<span data-ttu-id="b04d1-290">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="b04d1-290">**New behavior**</span></span>

<span data-ttu-id="b04d1-291">Теперь EF Core содержит собственное средство для преобразования во множественное число на основе библиотеки [Humanizer](https://github.com/Humanizr/Humanizer).</span><span class="sxs-lookup"><span data-stu-id="b04d1-291">EF Core now includes a pluralizer that uses the [Humanizer](https://github.com/Humanizr/Humanizer) library.</span></span> <span data-ttu-id="b04d1-292">Именно эту библиотеку использует Visual Studio, рекомендуя имена переменных.</span><span class="sxs-lookup"><span data-stu-id="b04d1-292">This is the same library Visual Studio uses to recommend variable names.</span></span>

<span data-ttu-id="b04d1-293">**Причина**</span><span class="sxs-lookup"><span data-stu-id="b04d1-293">**Why**</span></span>

<span data-ttu-id="b04d1-294">Использование форм множественного числа для свойств коллекций и единственного числа для типов и ссылочных свойств считается идиоматичным в .NET.</span><span class="sxs-lookup"><span data-stu-id="b04d1-294">Using plural forms of words for collection properties and singular forms for types and reference properties is idiomatic in .NET.</span></span>

<span data-ttu-id="b04d1-295">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="b04d1-295">**Mitigations**</span></span>

<span data-ttu-id="b04d1-296">Чтобы отключить средство для преобразования во множественное число, укажите параметр `--no-pluralize` для `dotnet ef dbcontext scaffold` или `-NoPluralize` для `Scaffold-DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b04d1-296">To disable the pluralizer, use the `--no-pluralize` option on `dotnet ef dbcontext scaffold` or the `-NoPluralize` switch on `Scaffold-DbContext`.</span></span>
