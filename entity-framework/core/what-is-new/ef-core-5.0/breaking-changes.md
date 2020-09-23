---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 09/09/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: 63fd1d1a01b7a72fd34bb9a0130191131306426c
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90070800"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="25150-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="25150-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="25150-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="25150-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="25150-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="25150-105">Summary</span></span>

| <span data-ttu-id="25150-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="25150-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="25150-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="25150-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="25150-108">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="25150-108">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="25150-109">Средний</span><span class="sxs-lookup"><span data-stu-id="25150-109">Medium</span></span>     |
| [<span data-ttu-id="25150-110">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="25150-110">Defining query is replaced with provider-specific methods</span></span>](#defining-query)                                                          | <span data-ttu-id="25150-111">Средний</span><span class="sxs-lookup"><span data-stu-id="25150-111">Medium</span></span>     |
| [<span data-ttu-id="25150-112">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="25150-112">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="25150-113">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-113">Low</span></span>        |
| [<span data-ttu-id="25150-114">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="25150-114">Cosmos: Partition key is now added to the primary key</span></span>](#cosmos-partition-key)                                                        | <span data-ttu-id="25150-115">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-115">Low</span></span>        |
| [<span data-ttu-id="25150-116">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="25150-116">Cosmos: `id` property renamed to `__id`</span></span>](#cosmos-id)                                                                                 | <span data-ttu-id="25150-117">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-117">Low</span></span>        |
| <span data-ttu-id="25150-118">[Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)</span><span class="sxs-lookup"><span data-stu-id="25150-118">[Cosmos: byte[] is now stored as a base64 string instead of a number array](#cosmos-byte)</span></span>                                             | <span data-ttu-id="25150-119">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-119">Low</span></span>        |
| [<span data-ttu-id="25150-120">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="25150-120">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>](#cosmos-metadata)                                                          | <span data-ttu-id="25150-121">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-121">Low</span></span>        |
| [<span data-ttu-id="25150-122">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="25150-122">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>](#non-added-generation) | <span data-ttu-id="25150-123">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-123">Low</span></span>        |
| [<span data-ttu-id="25150-124">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="25150-124">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="25150-125">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-125">Low</span></span>        |
| [<span data-ttu-id="25150-126">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="25150-126">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="25150-127">Низкий</span><span class="sxs-lookup"><span data-stu-id="25150-127">Low</span></span>        |

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="25150-128">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="25150-128">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="25150-129">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="25150-129">Tracking Issue #14257</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

<span data-ttu-id="25150-130">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-130">**Old behavior**</span></span>

<span data-ttu-id="25150-131">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="25150-131">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="25150-132">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="25150-132">However, it only ever affected database creation.</span></span> <span data-ttu-id="25150-133">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="25150-133">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="25150-134">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="25150-134">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span></span>

<span data-ttu-id="25150-135">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-135">**New behavior**</span></span>

<span data-ttu-id="25150-136">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="25150-136">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="25150-137">Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="25150-137">This API matches more closely to the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

<span data-ttu-id="25150-138">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-138">**Why**</span></span>

<span data-ttu-id="25150-139">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="25150-139">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

<span data-ttu-id="25150-140">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-140">**Mitigations**</span></span>

<span data-ttu-id="25150-141">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="25150-141">Use `HasColumnType` to specify the dimension:</span></span>

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

### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="25150-142">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="25150-142">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="25150-143">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="25150-143">Tracking Issue #17286</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17286)

<span data-ttu-id="25150-144">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-144">**Old behavior**</span></span>

<span data-ttu-id="25150-145">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="25150-145">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="25150-146">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="25150-146">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

<span data-ttu-id="25150-147">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-147">**New behavior**</span></span>

<span data-ttu-id="25150-148">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="25150-148">With the added support for required dependents, it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="25150-149">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="25150-149">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

<span data-ttu-id="25150-150">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-150">**Why**</span></span>

<span data-ttu-id="25150-151">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="25150-151">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

<span data-ttu-id="25150-152">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-152">**Mitigations**</span></span>

<span data-ttu-id="25150-153">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="25150-153">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="cosmos-partition-key"></a>

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a><span data-ttu-id="25150-154">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="25150-154">Cosmos: Partition key is now added to the primary key</span></span>

[<span data-ttu-id="25150-155">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="25150-155">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="25150-156">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-156">**Old behavior**</span></span>

<span data-ttu-id="25150-157">Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.</span><span class="sxs-lookup"><span data-stu-id="25150-157">The partition key property was only added to the alternate key that includes `id`.</span></span>

<span data-ttu-id="25150-158">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-158">**New behavior**</span></span>

<span data-ttu-id="25150-159">В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="25150-159">The partition key property is now also added to the primary key by convention.</span></span>

<span data-ttu-id="25150-160">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-160">**Why**</span></span>

<span data-ttu-id="25150-161">Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="25150-161">This change makes the model better aligned with Azure Cosmos DB semantics and improves the performance of `Find` and some queries.</span></span>

<span data-ttu-id="25150-162">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-162">**Mitigations**</span></span>

<span data-ttu-id="25150-163">Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="25150-163">To prevent the partition key property to be added to the primary key, configure it in `OnModelCreating`.</span></span>

```cs
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a><span data-ttu-id="25150-164">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="25150-164">Cosmos: `id` property renamed to `__id`</span></span>

[<span data-ttu-id="25150-165">Отслеживание вопроса № 17751</span><span class="sxs-lookup"><span data-stu-id="25150-165">Tracking Issue #17751</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17751)

<span data-ttu-id="25150-166">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-166">**Old behavior**</span></span>

<span data-ttu-id="25150-167">Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.</span><span class="sxs-lookup"><span data-stu-id="25150-167">The shadow property mapped to the `id` JSON property was also named `id`.</span></span>

<span data-ttu-id="25150-168">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-168">**New behavior**</span></span>

<span data-ttu-id="25150-169">Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.</span><span class="sxs-lookup"><span data-stu-id="25150-169">The shadow property created by convention is now named `__id`.</span></span>

<span data-ttu-id="25150-170">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-170">**Why**</span></span>

<span data-ttu-id="25150-171">Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.</span><span class="sxs-lookup"><span data-stu-id="25150-171">This change makes it less likely that the `id` property clashes with an existing property on the entity type.</span></span>

<span data-ttu-id="25150-172">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-172">**Mitigations**</span></span>

<span data-ttu-id="25150-173">Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="25150-173">To go back to the 3.x behavior, configure the `id` property in `OnModelCreating`.</span></span>

```cs
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a><span data-ttu-id="25150-174">Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива</span><span class="sxs-lookup"><span data-stu-id="25150-174">Cosmos: byte[] is now stored as a base64 string instead of a number array</span></span>

[<span data-ttu-id="25150-175">Отслеживание вопроса № 17306</span><span class="sxs-lookup"><span data-stu-id="25150-175">Tracking Issue #17306</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17306)

<span data-ttu-id="25150-176">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-176">**Old behavior**</span></span>

<span data-ttu-id="25150-177">Свойства типа byte[] хранились в виде числового массива.</span><span class="sxs-lookup"><span data-stu-id="25150-177">Properties of type byte[] were stored as a number array.</span></span>

<span data-ttu-id="25150-178">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-178">**New behavior**</span></span>

<span data-ttu-id="25150-179">Свойства типа byte[] теперь хранятся в виде строки base64.</span><span class="sxs-lookup"><span data-stu-id="25150-179">Properties of type byte[] are now stored as a base64 string.</span></span>

<span data-ttu-id="25150-180">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-180">**Why**</span></span>

<span data-ttu-id="25150-181">Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.</span><span class="sxs-lookup"><span data-stu-id="25150-181">This representation of byte[] aligns better with expectations and is the default behavior of the major JSON serialization libraries.</span></span>

<span data-ttu-id="25150-182">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-182">**Mitigations**</span></span>

<span data-ttu-id="25150-183">Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки.</span><span class="sxs-lookup"><span data-stu-id="25150-183">Existing data stored as number arrays will still be queried correctly, but currently there isn't a supported way to change back the insert behavior.</span></span> <span data-ttu-id="25150-184">Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/aspnet/EntityFrameworkCore/issues/17306)</span><span class="sxs-lookup"><span data-stu-id="25150-184">If this limitation is blocking your scenario, comment on [this issue](https://github.com/aspnet/EntityFrameworkCore/issues/17306)</span></span>

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a><span data-ttu-id="25150-185">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="25150-185">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>

[<span data-ttu-id="25150-186">Отслеживание вопроса № 17874</span><span class="sxs-lookup"><span data-stu-id="25150-186">Tracking Issue #17874</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/17874)

<span data-ttu-id="25150-187">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-187">**Old behavior**</span></span>

<span data-ttu-id="25150-188">Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="25150-188">Previously the extension methods were called `GetPropertyName` and `SetPropertyName`</span></span>

<span data-ttu-id="25150-189">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-189">**New behavior**</span></span>

<span data-ttu-id="25150-190">Этот интерфейс API устарел, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="25150-190">The old API was obsoleted and new methods added: `GetJsonPropertyName`, `SetJsonPropertyName`</span></span>

<span data-ttu-id="25150-191">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-191">**Why**</span></span>

<span data-ttu-id="25150-192">Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.</span><span class="sxs-lookup"><span data-stu-id="25150-192">This change removes the ambiguity around what these methods are configuring.</span></span>

<span data-ttu-id="25150-193">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-193">**Mitigations**</span></span>

<span data-ttu-id="25150-194">Используйте новый интерфейс API или временно отключите предупреждения об устаревших возможностях.</span><span class="sxs-lookup"><span data-stu-id="25150-194">Use the new API or temporarily suspend the obsolete warnings.</span></span>

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="25150-195">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="25150-195">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>

[<span data-ttu-id="25150-196">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="25150-196">Tracking Issue #15289</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

<span data-ttu-id="25150-197">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-197">**Old behavior**</span></span>

<span data-ttu-id="25150-198">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="25150-198">Value generators were only called when the entity state changed to Added.</span></span>

<span data-ttu-id="25150-199">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-199">**New behavior**</span></span>

<span data-ttu-id="25150-200">Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="25150-200">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated, or Deleted and the property contains the default values.</span></span>

<span data-ttu-id="25150-201">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-201">**Why**</span></span>

<span data-ttu-id="25150-202">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="25150-202">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

<span data-ttu-id="25150-203">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-203">**Mitigations**</span></span>

<span data-ttu-id="25150-204">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="25150-204">To prevent the value generator from being called, assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>

### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="25150-205">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="25150-205">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="25150-206">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="25150-206">Tracking Issue #20305</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/20305)

<span data-ttu-id="25150-207">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-207">**Old behavior**</span></span>

<span data-ttu-id="25150-208">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="25150-208">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

<span data-ttu-id="25150-209">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-209">**New behavior**</span></span>

<span data-ttu-id="25150-210">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="25150-210">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="25150-211">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="25150-211">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

<span data-ttu-id="25150-212">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-212">**Why**</span></span>

<span data-ttu-id="25150-213">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="25150-213">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="25150-214">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="25150-214">Using it to find differences is faster and more accurate.</span></span>

<span data-ttu-id="25150-215">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-215">**Mitigations**</span></span>

<span data-ttu-id="25150-216">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="25150-216">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

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

<span data-ttu-id="25150-217">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="25150-217">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="read-only-discriminators"></a>

### <a name="discriminators-are-read-only"></a><span data-ttu-id="25150-218">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="25150-218">Discriminators are read-only</span></span>

[<span data-ttu-id="25150-219">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="25150-219">Tracking Issue #21154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/21154)

<span data-ttu-id="25150-220">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-220">**Old behavior**</span></span>

<span data-ttu-id="25150-221">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="25150-221">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

<span data-ttu-id="25150-222">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-222">**New behavior**</span></span>

<span data-ttu-id="25150-223">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="25150-223">An exception will be throws in the above case.</span></span>

<span data-ttu-id="25150-224">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-224">**Why**</span></span>

<span data-ttu-id="25150-225">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="25150-225">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state, which might result in unexpected behavior.</span></span>

<span data-ttu-id="25150-226">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-226">**Mitigations**</span></span>

<span data-ttu-id="25150-227">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="25150-227">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges`, the discriminator can be made mutable:</span></span>

```cs
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a><span data-ttu-id="25150-228">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="25150-228">Defining query is replaced with provider-specific methods</span></span>

[<span data-ttu-id="25150-229">Отслеживание вопроса № 18903</span><span class="sxs-lookup"><span data-stu-id="25150-229">Tracking Issue #18903</span></span>](https://github.com/dotnet/efcore/issues/18903)

<span data-ttu-id="25150-230">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-230">**Old behavior**</span></span>

<span data-ttu-id="25150-231">Типы сущностей сопоставлялись с определяющими запросами на уровне ядра.</span><span class="sxs-lookup"><span data-stu-id="25150-231">Entity types were mapped to defining queries at the Core level.</span></span> <span data-ttu-id="25150-232">Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.</span><span class="sxs-lookup"><span data-stu-id="25150-232">Anytime the entity type was used in the query root of the entity type was replaced by the defining query for any provider.</span></span>

<span data-ttu-id="25150-233">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="25150-233">**New behavior**</span></span>

<span data-ttu-id="25150-234">Интерфейсы API для определяющего запроса стали нерекомендуемыми.</span><span class="sxs-lookup"><span data-stu-id="25150-234">APIs for defining query are deprecated.</span></span> <span data-ttu-id="25150-235">Появились новые интерфейсы API для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="25150-235">New provider-specific APIs were introduced.</span></span>

<span data-ttu-id="25150-236">**Причина**</span><span class="sxs-lookup"><span data-stu-id="25150-236">**Why**</span></span>

<span data-ttu-id="25150-237">Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.</span><span class="sxs-lookup"><span data-stu-id="25150-237">While defining queries were implemented as replacement query whenever query root is used in the query, it had a few issues:</span></span>

- <span data-ttu-id="25150-238">Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.</span><span class="sxs-lookup"><span data-stu-id="25150-238">If defining query is projecting entity type using `new { ... }` in `Select` method, then identifying that as an entity required additional work and made it inconsistent with how EF Core treats nominal types in the query.</span></span>
- <span data-ttu-id="25150-239">Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.</span><span class="sxs-lookup"><span data-stu-id="25150-239">For relational providers `FromSql` is still needed to pass the SQL string in LINQ expression form.</span></span>

<span data-ttu-id="25150-240">Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных).</span><span class="sxs-lookup"><span data-stu-id="25150-240">Initially defining queries were introduced as client-side views to be used with In-Memory provider for keyless entities (similar to database views in relational databases).</span></span> <span data-ttu-id="25150-241">Такое определение упрощает тестирование приложения с базой данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="25150-241">Such definition makes it easy to test application against in-memory database.</span></span> <span data-ttu-id="25150-242">Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание.</span><span class="sxs-lookup"><span data-stu-id="25150-242">Afterwards they became broadly applicable, which was useful but brought inconsistent and hard to understand behavior.</span></span> <span data-ttu-id="25150-243">Поэтому мы решили упростить принцип их работы.</span><span class="sxs-lookup"><span data-stu-id="25150-243">So we decided to simplify the concept.</span></span> <span data-ttu-id="25150-244">Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому.</span><span class="sxs-lookup"><span data-stu-id="25150-244">We made LINQ based defining query exclusive to In-Memory provider and treat them differently.</span></span> <span data-ttu-id="25150-245">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).</span><span class="sxs-lookup"><span data-stu-id="25150-245">For more information, [see this issue](https://github.com/dotnet/efcore/issues/20023).</span></span>

<span data-ttu-id="25150-246">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="25150-246">**Mitigations**</span></span>

<span data-ttu-id="25150-247">Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="25150-247">For relational providers, use `ToSqlQuery` method in `OnModelCreating` and pass in a SQL string to use for the entity type.</span></span>
<span data-ttu-id="25150-248">Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="25150-248">For the In-Memory provider, use `ToInMemoryQuery` method in `OnModelCreating` and pass in a LINQ query to use for the entity type.</span></span>
