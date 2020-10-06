---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 09/09/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: 8e9df4e2ff81e20cf5a36855247c5aff89ea2394
ms.sourcegitcommit: c0e6a00b64c2dcd8acdc0fe6d1b47703405cdf09
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91210371"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="68179-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="68179-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="68179-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="68179-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="68179-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="68179-105">Summary</span></span>

| <span data-ttu-id="68179-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="68179-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="68179-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="68179-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="68179-108">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="68179-108">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="68179-109">Средний</span><span class="sxs-lookup"><span data-stu-id="68179-109">Medium</span></span>     |
| [<span data-ttu-id="68179-110">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="68179-110">Defining query is replaced with provider-specific methods</span></span>](#defining-query)                                                          | <span data-ttu-id="68179-111">Средний</span><span class="sxs-lookup"><span data-stu-id="68179-111">Medium</span></span>     |
| [<span data-ttu-id="68179-112">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="68179-112">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="68179-113">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-113">Low</span></span>        |
| [<span data-ttu-id="68179-114">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="68179-114">Cosmos: Partition key is now added to the primary key</span></span>](#cosmos-partition-key)                                                        | <span data-ttu-id="68179-115">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-115">Low</span></span>        |
| [<span data-ttu-id="68179-116">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="68179-116">Cosmos: `id` property renamed to `__id`</span></span>](#cosmos-id)                                                                                 | <span data-ttu-id="68179-117">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-117">Low</span></span>        |
| <span data-ttu-id="68179-118">[Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)</span><span class="sxs-lookup"><span data-stu-id="68179-118">[Cosmos: byte[] is now stored as a base64 string instead of a number array](#cosmos-byte)</span></span>                                             | <span data-ttu-id="68179-119">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-119">Low</span></span>        |
| [<span data-ttu-id="68179-120">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="68179-120">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>](#cosmos-metadata)                                                          | <span data-ttu-id="68179-121">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-121">Low</span></span>        |
| [<span data-ttu-id="68179-122">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="68179-122">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>](#non-added-generation) | <span data-ttu-id="68179-123">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-123">Low</span></span>        |
| [<span data-ttu-id="68179-124">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="68179-124">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="68179-125">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-125">Low</span></span>        |
| [<span data-ttu-id="68179-126">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="68179-126">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="68179-127">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-127">Low</span></span>        |
| [<span data-ttu-id="68179-128">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="68179-128">Provider-specific EF.Functions methods throw for InMemory provider</span></span>](#no-client-methods)                                              | <span data-ttu-id="68179-129">Низкий</span><span class="sxs-lookup"><span data-stu-id="68179-129">Low</span></span>        |

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="68179-130">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="68179-130">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="68179-131">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="68179-131">Tracking Issue #14257</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

<span data-ttu-id="68179-132">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-132">**Old behavior**</span></span>

<span data-ttu-id="68179-133">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="68179-133">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="68179-134">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="68179-134">However, it only ever affected database creation.</span></span> <span data-ttu-id="68179-135">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="68179-135">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="68179-136">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="68179-136">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span></span>

<span data-ttu-id="68179-137">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-137">**New behavior**</span></span>

<span data-ttu-id="68179-138">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="68179-138">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="68179-139">Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="68179-139">This API matches more closely to the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

<span data-ttu-id="68179-140">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-140">**Why**</span></span>

<span data-ttu-id="68179-141">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="68179-141">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

<span data-ttu-id="68179-142">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-142">**Mitigations**</span></span>

<span data-ttu-id="68179-143">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="68179-143">Use `HasColumnType` to specify the dimension:</span></span>

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

### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="68179-144">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="68179-144">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="68179-145">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="68179-145">Tracking Issue #17286</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17286)

<span data-ttu-id="68179-146">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-146">**Old behavior**</span></span>

<span data-ttu-id="68179-147">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="68179-147">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="68179-148">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="68179-148">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

<span data-ttu-id="68179-149">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-149">**New behavior**</span></span>

<span data-ttu-id="68179-150">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="68179-150">With the added support for required dependents, it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="68179-151">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="68179-151">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

<span data-ttu-id="68179-152">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-152">**Why**</span></span>

<span data-ttu-id="68179-153">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="68179-153">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

<span data-ttu-id="68179-154">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-154">**Mitigations**</span></span>

<span data-ttu-id="68179-155">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="68179-155">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="cosmos-partition-key"></a>

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a><span data-ttu-id="68179-156">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="68179-156">Cosmos: Partition key is now added to the primary key</span></span>

[<span data-ttu-id="68179-157">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="68179-157">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="68179-158">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-158">**Old behavior**</span></span>

<span data-ttu-id="68179-159">Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.</span><span class="sxs-lookup"><span data-stu-id="68179-159">The partition key property was only added to the alternate key that includes `id`.</span></span>

<span data-ttu-id="68179-160">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-160">**New behavior**</span></span>

<span data-ttu-id="68179-161">В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="68179-161">The partition key property is now also added to the primary key by convention.</span></span>

<span data-ttu-id="68179-162">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-162">**Why**</span></span>

<span data-ttu-id="68179-163">Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="68179-163">This change makes the model better aligned with Azure Cosmos DB semantics and improves the performance of `Find` and some queries.</span></span>

<span data-ttu-id="68179-164">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-164">**Mitigations**</span></span>

<span data-ttu-id="68179-165">Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="68179-165">To prevent the partition key property to be added to the primary key, configure it in `OnModelCreating`.</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a><span data-ttu-id="68179-166">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="68179-166">Cosmos: `id` property renamed to `__id`</span></span>

[<span data-ttu-id="68179-167">Отслеживание вопроса № 17751</span><span class="sxs-lookup"><span data-stu-id="68179-167">Tracking Issue #17751</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17751)

<span data-ttu-id="68179-168">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-168">**Old behavior**</span></span>

<span data-ttu-id="68179-169">Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.</span><span class="sxs-lookup"><span data-stu-id="68179-169">The shadow property mapped to the `id` JSON property was also named `id`.</span></span>

<span data-ttu-id="68179-170">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-170">**New behavior**</span></span>

<span data-ttu-id="68179-171">Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.</span><span class="sxs-lookup"><span data-stu-id="68179-171">The shadow property created by convention is now named `__id`.</span></span>

<span data-ttu-id="68179-172">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-172">**Why**</span></span>

<span data-ttu-id="68179-173">Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.</span><span class="sxs-lookup"><span data-stu-id="68179-173">This change makes it less likely that the `id` property clashes with an existing property on the entity type.</span></span>

<span data-ttu-id="68179-174">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-174">**Mitigations**</span></span>

<span data-ttu-id="68179-175">Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="68179-175">To go back to the 3.x behavior, configure the `id` property in `OnModelCreating`.</span></span>

```cs
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a><span data-ttu-id="68179-176">Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива</span><span class="sxs-lookup"><span data-stu-id="68179-176">Cosmos: byte[] is now stored as a base64 string instead of a number array</span></span>

[<span data-ttu-id="68179-177">Отслеживание вопроса № 17306</span><span class="sxs-lookup"><span data-stu-id="68179-177">Tracking Issue #17306</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17306)

<span data-ttu-id="68179-178">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-178">**Old behavior**</span></span>

<span data-ttu-id="68179-179">Свойства типа byte[] хранились в виде числового массива.</span><span class="sxs-lookup"><span data-stu-id="68179-179">Properties of type byte[] were stored as a number array.</span></span>

<span data-ttu-id="68179-180">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-180">**New behavior**</span></span>

<span data-ttu-id="68179-181">Свойства типа byte[] теперь хранятся в виде строки base64.</span><span class="sxs-lookup"><span data-stu-id="68179-181">Properties of type byte[] are now stored as a base64 string.</span></span>

<span data-ttu-id="68179-182">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-182">**Why**</span></span>

<span data-ttu-id="68179-183">Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.</span><span class="sxs-lookup"><span data-stu-id="68179-183">This representation of byte[] aligns better with expectations and is the default behavior of the major JSON serialization libraries.</span></span>

<span data-ttu-id="68179-184">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-184">**Mitigations**</span></span>

<span data-ttu-id="68179-185">Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки.</span><span class="sxs-lookup"><span data-stu-id="68179-185">Existing data stored as number arrays will still be queried correctly, but currently there isn't a supported way to change back the insert behavior.</span></span> <span data-ttu-id="68179-186">Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/aspnet/EntityFrameworkCore/issues/17306)</span><span class="sxs-lookup"><span data-stu-id="68179-186">If this limitation is blocking your scenario, comment on [this issue](https://github.com/aspnet/EntityFrameworkCore/issues/17306)</span></span>

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a><span data-ttu-id="68179-187">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="68179-187">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>

[<span data-ttu-id="68179-188">Отслеживание вопроса № 17874</span><span class="sxs-lookup"><span data-stu-id="68179-188">Tracking Issue #17874</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17874)

<span data-ttu-id="68179-189">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-189">**Old behavior**</span></span>

<span data-ttu-id="68179-190">Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="68179-190">Previously the extension methods were called `GetPropertyName` and `SetPropertyName`</span></span>

<span data-ttu-id="68179-191">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-191">**New behavior**</span></span>

<span data-ttu-id="68179-192">Старый интерфейс API был удален, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="68179-192">The old API was removed and new methods added: `GetJsonPropertyName`, `SetJsonPropertyName`</span></span>

<span data-ttu-id="68179-193">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-193">**Why**</span></span>

<span data-ttu-id="68179-194">Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.</span><span class="sxs-lookup"><span data-stu-id="68179-194">This change removes the ambiguity around what these methods are configuring.</span></span>

<span data-ttu-id="68179-195">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-195">**Mitigations**</span></span>

<span data-ttu-id="68179-196">Используйте новый API.</span><span class="sxs-lookup"><span data-stu-id="68179-196">Use the new API.</span></span>

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="68179-197">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="68179-197">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>

[<span data-ttu-id="68179-198">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="68179-198">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="68179-199">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-199">**Old behavior**</span></span>

<span data-ttu-id="68179-200">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="68179-200">Value generators were only called when the entity state changed to Added.</span></span>

<span data-ttu-id="68179-201">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-201">**New behavior**</span></span>

<span data-ttu-id="68179-202">Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="68179-202">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated, or Deleted and the property contains the default values.</span></span>

<span data-ttu-id="68179-203">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-203">**Why**</span></span>

<span data-ttu-id="68179-204">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="68179-204">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

<span data-ttu-id="68179-205">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-205">**Mitigations**</span></span>

<span data-ttu-id="68179-206">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="68179-206">To prevent the value generator from being called, assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>

### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="68179-207">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="68179-207">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="68179-208">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="68179-208">Tracking Issue #20305</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/20305)

<span data-ttu-id="68179-209">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-209">**Old behavior**</span></span>

<span data-ttu-id="68179-210">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="68179-210">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

<span data-ttu-id="68179-211">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-211">**New behavior**</span></span>

<span data-ttu-id="68179-212">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="68179-212">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="68179-213">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="68179-213">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

<span data-ttu-id="68179-214">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-214">**Why**</span></span>

<span data-ttu-id="68179-215">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="68179-215">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="68179-216">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="68179-216">Using it to find differences is faster and more accurate.</span></span>

<span data-ttu-id="68179-217">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-217">**Mitigations**</span></span>

<span data-ttu-id="68179-218">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="68179-218">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

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

<span data-ttu-id="68179-219">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="68179-219">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="read-only-discriminators"></a>

### <a name="discriminators-are-read-only"></a><span data-ttu-id="68179-220">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="68179-220">Discriminators are read-only</span></span>

[<span data-ttu-id="68179-221">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="68179-221">Tracking Issue #21154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/21154)

<span data-ttu-id="68179-222">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-222">**Old behavior**</span></span>

<span data-ttu-id="68179-223">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="68179-223">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

<span data-ttu-id="68179-224">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-224">**New behavior**</span></span>

<span data-ttu-id="68179-225">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="68179-225">An exception will be throws in the above case.</span></span>

<span data-ttu-id="68179-226">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-226">**Why**</span></span>

<span data-ttu-id="68179-227">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="68179-227">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state, which might result in unexpected behavior.</span></span>

<span data-ttu-id="68179-228">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-228">**Mitigations**</span></span>

<span data-ttu-id="68179-229">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="68179-229">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges`, the discriminator can be made mutable:</span></span>

```cs
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a><span data-ttu-id="68179-230">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="68179-230">Defining query is replaced with provider-specific methods</span></span>

[<span data-ttu-id="68179-231">Отслеживание вопроса № 18903</span><span class="sxs-lookup"><span data-stu-id="68179-231">Tracking Issue #18903</span></span>](https://github.com/dotnet/efcore/issues/18903)

<span data-ttu-id="68179-232">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-232">**Old behavior**</span></span>

<span data-ttu-id="68179-233">Типы сущностей сопоставлялись с определяющими запросами на уровне ядра.</span><span class="sxs-lookup"><span data-stu-id="68179-233">Entity types were mapped to defining queries at the Core level.</span></span> <span data-ttu-id="68179-234">Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.</span><span class="sxs-lookup"><span data-stu-id="68179-234">Anytime the entity type was used in the query root of the entity type was replaced by the defining query for any provider.</span></span>

<span data-ttu-id="68179-235">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-235">**New behavior**</span></span>

<span data-ttu-id="68179-236">Интерфейсы API для определяющего запроса стали нерекомендуемыми.</span><span class="sxs-lookup"><span data-stu-id="68179-236">APIs for defining query are deprecated.</span></span> <span data-ttu-id="68179-237">Появились новые интерфейсы API для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="68179-237">New provider-specific APIs were introduced.</span></span>

<span data-ttu-id="68179-238">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-238">**Why**</span></span>

<span data-ttu-id="68179-239">Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.</span><span class="sxs-lookup"><span data-stu-id="68179-239">While defining queries were implemented as replacement query whenever query root is used in the query, it had a few issues:</span></span>

- <span data-ttu-id="68179-240">Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.</span><span class="sxs-lookup"><span data-stu-id="68179-240">If defining query is projecting entity type using `new { ... }` in `Select` method, then identifying that as an entity required additional work and made it inconsistent with how EF Core treats nominal types in the query.</span></span>
- <span data-ttu-id="68179-241">Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.</span><span class="sxs-lookup"><span data-stu-id="68179-241">For relational providers `FromSql` is still needed to pass the SQL string in LINQ expression form.</span></span>

<span data-ttu-id="68179-242">Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных).</span><span class="sxs-lookup"><span data-stu-id="68179-242">Initially defining queries were introduced as client-side views to be used with In-Memory provider for keyless entities (similar to database views in relational databases).</span></span> <span data-ttu-id="68179-243">Такое определение упрощает тестирование приложения с базой данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="68179-243">Such definition makes it easy to test application against in-memory database.</span></span> <span data-ttu-id="68179-244">Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание.</span><span class="sxs-lookup"><span data-stu-id="68179-244">Afterwards they became broadly applicable, which was useful but brought inconsistent and hard to understand behavior.</span></span> <span data-ttu-id="68179-245">Поэтому мы решили упростить принцип их работы.</span><span class="sxs-lookup"><span data-stu-id="68179-245">So we decided to simplify the concept.</span></span> <span data-ttu-id="68179-246">Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому.</span><span class="sxs-lookup"><span data-stu-id="68179-246">We made LINQ based defining query exclusive to In-Memory provider and treat them differently.</span></span> <span data-ttu-id="68179-247">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).</span><span class="sxs-lookup"><span data-stu-id="68179-247">For more information, [see this issue](https://github.com/dotnet/efcore/issues/20023).</span></span>

<span data-ttu-id="68179-248">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-248">**Mitigations**</span></span>

<span data-ttu-id="68179-249">Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="68179-249">For relational providers, use `ToSqlQuery` method in `OnModelCreating` and pass in a SQL string to use for the entity type.</span></span>
<span data-ttu-id="68179-250">Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="68179-250">For the In-Memory provider, use `ToInMemoryQuery` method in `OnModelCreating` and pass in a LINQ query to use for the entity type.</span></span>

<a name="no-client-methods"></a>

### <a name="provider-specific-effunctions-methods-throw-for-inmemory-provider"></a><span data-ttu-id="68179-251">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="68179-251">Provider-specific EF.Functions methods throw for InMemory provider</span></span>

[<span data-ttu-id="68179-252">Отслеживание вопроса № 20294</span><span class="sxs-lookup"><span data-stu-id="68179-252">Tracking Issue #20294</span></span>](https://github.com/dotnet/efcore/issues/20294)

<span data-ttu-id="68179-253">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-253">**Old behavior**</span></span>

<span data-ttu-id="68179-254">Относящиеся к конкретному поставщику методы EF.Functions содержали реализацию для выполнения на стороне клиента, что позволяло им выполняться в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="68179-254">Provider-specific EF.Functions methods contained implementation for client execution, which allowed them to be executed on the InMemory provider.</span></span> <span data-ttu-id="68179-255">Например, `EF.Functions.DateDiffDay` — это относящийся к SQL Server метод, который работал в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="68179-255">For example, `EF.Functions.DateDiffDay` is a Sql Server specific method, which worked on InMemory provider.</span></span>

<span data-ttu-id="68179-256">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="68179-256">**New behavior**</span></span>

<span data-ttu-id="68179-257">Методы, относящиеся к конкретному поставщику, были изменены так, чтобы в теле метода создавалось исключение, блокирующее вычисление метода на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="68179-257">Provider-specific methods have been updated to throw an exception in their method body to block evaluating them on client side.</span></span>

<span data-ttu-id="68179-258">**Причина**</span><span class="sxs-lookup"><span data-stu-id="68179-258">**Why**</span></span>

<span data-ttu-id="68179-259">Методы, относящиеся к конкретному поставщику, сопоставляются с функцией базы данных.</span><span class="sxs-lookup"><span data-stu-id="68179-259">Provider-specific methods map to a database function.</span></span> <span data-ttu-id="68179-260">Вычисление, выполняемое сопоставленной функцией базы данных, не всегда может реплицироваться на стороне клиента в LINQ.</span><span class="sxs-lookup"><span data-stu-id="68179-260">The computation done by the mapped database function can't always be replicated on the client side in LINQ.</span></span> <span data-ttu-id="68179-261">Поэтому результаты, полученные при выполнении одного и того же метода на сервере и в клиенте, могут различаться.</span><span class="sxs-lookup"><span data-stu-id="68179-261">It may cause the result from the server to differ when executing the same method on client.</span></span> <span data-ttu-id="68179-262">Так как эти методы используются в LINQ для преобразования в определенные функции базы данных, их не нужно вычислять на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="68179-262">Since these methods are used in LINQ to translate to specific database functions, they don't need to be evaluated on client side.</span></span> <span data-ttu-id="68179-263">Так как поставщик InMemory — это другая *база данных*, эти методы недоступны для него.</span><span class="sxs-lookup"><span data-stu-id="68179-263">As InMemory provider is a different *database*, these methods aren't available for this provider.</span></span> <span data-ttu-id="68179-264">При попытке выполнить их для поставщика InMemory или любого другого поставщика, который не преобразует эти методы, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="68179-264">Trying to execute them for InMemory provider, or any other provider that doesn't translate these methods, throws an exception.</span></span>

<span data-ttu-id="68179-265">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="68179-265">**Mitigations**</span></span>

<span data-ttu-id="68179-266">Так как точно воссоздать поведение функций базы данных невозможно, содержащие их запросы следует тестировать на основе базы данных того же типа, что и рабочая база данных.</span><span class="sxs-lookup"><span data-stu-id="68179-266">Since there's no way to mimic behavior of database functions accurately, you should test the queries containing them against same kind of database as in production.</span></span>
