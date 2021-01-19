---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 11/07/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: 4a463e785edaceaf5dd96164c39e2cc9b5f86de4
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128749"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="d6f2c-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="d6f2c-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="d6f2c-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="d6f2c-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="d6f2c-105">Summary</span></span>

| <span data-ttu-id="d6f2c-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="d6f2c-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="d6f2c-108">EF Core 5.0 не поддерживает платформу .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d6f2c-108">EF Core 5.0 does not support .NET Framework</span></span>](#netstandard21)                                                                         | <span data-ttu-id="d6f2c-109">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-109">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-110">IProperty.GetColumnName() считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="d6f2c-110">IProperty.GetColumnName() is now obsolete</span></span>](#getcolumnname-obsolete)                                                                  | <span data-ttu-id="d6f2c-111">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-111">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-112">Для десятичных значений требуется указывать значения точности и масштаба</span><span class="sxs-lookup"><span data-stu-id="d6f2c-112">Precision and scale are required for decimals</span></span>](#decimals)                                                                            | <span data-ttu-id="d6f2c-113">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-113">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-114">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="d6f2c-114">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="d6f2c-115">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-115">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-116">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="d6f2c-116">Defining query is replaced with provider-specific methods</span></span>](#defining-query)                                                          | <span data-ttu-id="d6f2c-117">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-117">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-118">Навигации по ссылке, которая не является пустой, не переопределяются запросами</span><span class="sxs-lookup"><span data-stu-id="d6f2c-118">Non-null reference navigations are not overwritten by queries</span></span>](#nonnullreferences)                                                   | <span data-ttu-id="d6f2c-119">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-119">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-120">ToView() по-разному обрабатывается миграциями</span><span class="sxs-lookup"><span data-stu-id="d6f2c-120">ToView() is treated differently by migrations</span></span>](#toview)                                                                              | <span data-ttu-id="d6f2c-121">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-121">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-122">ToTable(null) помечает тип сущности как не сопоставленный с таблицей</span><span class="sxs-lookup"><span data-stu-id="d6f2c-122">ToTable(null) marks the entity type as not mapped to a table</span></span>](#totable)                                                              | <span data-ttu-id="d6f2c-123">Средний</span><span class="sxs-lookup"><span data-stu-id="d6f2c-123">Medium</span></span>     |
| [<span data-ttu-id="d6f2c-124">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-124">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="d6f2c-125">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-125">Low</span></span>        |
| [<span data-ttu-id="d6f2c-126">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="d6f2c-126">Cosmos: Partition key is now added to the primary key</span></span>](#cosmos-partition-key)                                                        | <span data-ttu-id="d6f2c-127">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-127">Low</span></span>        |
| [<span data-ttu-id="d6f2c-128">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="d6f2c-128">Cosmos: `id` property renamed to `__id`</span></span>](#cosmos-id)                                                                                 | <span data-ttu-id="d6f2c-129">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-129">Low</span></span>        |
| <span data-ttu-id="d6f2c-130">[Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)</span><span class="sxs-lookup"><span data-stu-id="d6f2c-130">[Cosmos: byte[] is now stored as a base64 string instead of a number array](#cosmos-byte)</span></span>                                             | <span data-ttu-id="d6f2c-131">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-131">Low</span></span>        |
| [<span data-ttu-id="d6f2c-132">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="d6f2c-132">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>](#cosmos-metadata)                                                          | <span data-ttu-id="d6f2c-133">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-133">Low</span></span>        |
| [<span data-ttu-id="d6f2c-134">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="d6f2c-134">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>](#non-added-generation) | <span data-ttu-id="d6f2c-135">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-135">Low</span></span>        |
| [<span data-ttu-id="d6f2c-136">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="d6f2c-136">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="d6f2c-137">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-137">Low</span></span>        |
| [<span data-ttu-id="d6f2c-138">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="d6f2c-138">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="d6f2c-139">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-139">Low</span></span>        |
| [<span data-ttu-id="d6f2c-140">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="d6f2c-140">Provider-specific EF.Functions methods throw for InMemory provider</span></span>](#no-client-methods)                                              | <span data-ttu-id="d6f2c-141">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-141">Low</span></span>        |
| [<span data-ttu-id="d6f2c-142">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="d6f2c-142">IndexBuilder.HasName is now obsolete</span></span>](#index-obsolete)                                                                               | <span data-ttu-id="d6f2c-143">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-143">Low</span></span>        |
| [<span data-ttu-id="d6f2c-144">Добавлено средство преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="d6f2c-144">A pluralizer is now included for scaffolding reverse engineered models</span></span>](#pluralizer)                                                 | <span data-ttu-id="d6f2c-145">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-145">Low</span></span>        |
| [<span data-ttu-id="d6f2c-146">INavigationBase заменяет INavigation в некоторых API, чтобы поддерживать пропуск навигации</span><span class="sxs-lookup"><span data-stu-id="d6f2c-146">INavigationBase replaces INavigation in some APIs to support skip navigations</span></span>](#inavigationbase)                                     | <span data-ttu-id="d6f2c-147">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-147">Low</span></span>        |
| <span data-ttu-id="d6f2c-148">[Некоторые запросы с помощью коррелированной коллекции, которые также используют `Distinct` или `GroupBy`, больше не поддерживаются](#collection-distinct-groupby).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-148">[Some queries with correlated collection that also use `Distinct` or `GroupBy` are no longer supported](#collection-distinct-groupby)</span></span> | <span data-ttu-id="d6f2c-149">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-149">Low</span></span>        |
| [<span data-ttu-id="d6f2c-150">Использование коллекции запрашиваемого типа в проекции не поддерживается</span><span class="sxs-lookup"><span data-stu-id="d6f2c-150">Using a collection of Queryable type in projection is not supported</span></span>](#queryable-projection)                                          | <span data-ttu-id="d6f2c-151">Низкий</span><span class="sxs-lookup"><span data-stu-id="d6f2c-151">Low</span></span>        |

## <a name="medium-impact-changes"></a><span data-ttu-id="d6f2c-152">Изменения средней степени влияния</span><span class="sxs-lookup"><span data-stu-id="d6f2c-152">Medium-impact changes</span></span>

<a name="netstandard21"></a>

### <a name="ef-core-50-does-not-support-net-framework"></a><span data-ttu-id="d6f2c-153">EF Core 5.0 не поддерживает платформу .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d6f2c-153">EF Core 5.0 does not support .NET Framework</span></span>

[<span data-ttu-id="d6f2c-154">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="d6f2c-154">Tracking Issue #15498</span></span>](https://github.com/dotnet/efcore/issues/15498)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-155">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-155">Old behavior</span></span>

<span data-ttu-id="d6f2c-156">EF Core 3.1 предназначается для платформы .NET Standard 2.0, которая поддерживается платформой .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-156">EF Core 3.1 targets .NET Standard 2.0, which is supported by .NET Framework.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-157">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-157">New behavior</span></span>

<span data-ttu-id="d6f2c-158">EF Core 5.0 предназначается для платформы .NET Standard 2.1, которая не поддерживается платформой .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-158">EF Core 5.0 targets .NET Standard 2.1, which is not supported by .NET Framework.</span></span> <span data-ttu-id="d6f2c-159">Это означает, что EF Core 5.0 нельзя использовать с приложениями .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-159">This means EF Core 5.0 cannot be used with .NET Framework applications.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-160">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-160">Why</span></span>

<span data-ttu-id="d6f2c-161">Это часть масштабного процесса между группами .NET, направленного на унификацию на базе единой целевой платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-161">This is part of the wider movement across .NET teams aimed at unification to a single .NET target framework.</span></span> <span data-ttu-id="d6f2c-162">Дополнительные сведения см. в разделе [Перспективы .NET Standard](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard/).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-162">For more information see [the future of .NET Standard](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard/).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-163">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-163">Mitigations</span></span>

<span data-ttu-id="d6f2c-164">В приложениях .NET Framework по-прежнему возможно использование выпуска EF Core 3.1, который рассчитан на [долгосрочную поддержку (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-164">.NET Framework applications can continue to use EF Core 3.1, which is a [long-term support (LTS) release](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span> <span data-ttu-id="d6f2c-165">Кроме того, можно обновить приложения для работы с .NET Core 2.1, .NET Core 3.1 или .NET 5, которые поддерживают .NET Standard 2.1.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-165">Alternately, applications can be updated to use .NET Core 2.1, .NET Core 3.1, or .NET 5, all of which support .NET Standard 2.1.</span></span>

<a name="getcolumnname-obsolete"></a>

### <a name="ipropertygetcolumnname-is-now-obsolete"></a><span data-ttu-id="d6f2c-166">IProperty.GetColumnName() считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="d6f2c-166">IProperty.GetColumnName() is now obsolete</span></span>

[<span data-ttu-id="d6f2c-167">Отслеживание проблемы № 2266</span><span class="sxs-lookup"><span data-stu-id="d6f2c-167">Tracking Issue #2266</span></span>](https://github.com/dotnet/efcore/issues/2266)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-168">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-168">Old behavior</span></span>

<span data-ttu-id="d6f2c-169">`GetColumnName()` возвращал имя столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-169">`GetColumnName()` returned the name of the column that a property is mapped to.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-170">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-170">New behavior</span></span>

<span data-ttu-id="d6f2c-171">`GetColumnName()` по-прежнему возвращает имя столбца, с которым сопоставлено свойство, но это поведение стало неоднозначным, так как EF Core 5 поддерживает TPT и одновременное сопоставление с представлением или функцией, где эти сопоставления могут использовать разные имена столбцов для одного и того же свойства.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-171">`GetColumnName()` still returns the name of a column that a property is mapped to, but this behavior is now ambiguous since EF Core 5 supports TPT and simultaneous mapping to a view or a function where these mappings could use different column names for the same property.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-172">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-172">Why</span></span>

<span data-ttu-id="d6f2c-173">Мы пометили этот метод как устаревший, чтобы помочь пользователям более точно перегружать <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName(Microsoft.EntityFrameworkCore.Metadata.IProperty,Microsoft.EntityFrameworkCore.Metadata.StoreObjectIdentifier@)>.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-173">We marked this method as obsolete to guide users to a more accurate overload - <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName(Microsoft.EntityFrameworkCore.Metadata.IProperty,Microsoft.EntityFrameworkCore.Metadata.StoreObjectIdentifier@)>.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-174">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-174">Mitigations</span></span>

<span data-ttu-id="d6f2c-175">Используйте следующий код, чтобы получить имя столбца для определенной таблицы:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-175">Use the following code to get the column name for a specific table:</span></span>

```csharp
var columnName = property.GetColumnName(StoreObjectIdentifier.Table("Users", null)));
```

<a name="decimals"></a>

### <a name="precision-and-scale-are-required-for-decimals"></a><span data-ttu-id="d6f2c-176">Для десятичных значений требуется указывать значения точности и масштаба</span><span class="sxs-lookup"><span data-stu-id="d6f2c-176">Precision and scale are required for decimals</span></span>

[<span data-ttu-id="d6f2c-177">Отслеживание проблемы № 19293</span><span class="sxs-lookup"><span data-stu-id="d6f2c-177">Tracking Issue #19293</span></span>](https://github.com/dotnet/efcore/issues/19293)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-178">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-178">Old behavior</span></span>

<span data-ttu-id="d6f2c-179">В EF Core, как правило, не устанавливались значения точности и масштаба для объектов <xref:Microsoft.Data.SqlClient.SqlParameter>.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-179">EF Core did not normally set precision and scale on <xref:Microsoft.Data.SqlClient.SqlParameter> objects.</span></span> <span data-ttu-id="d6f2c-180">Это означает, что полные значения точности и масштаба отправлялись в SQL Server, где на этом этапе выполнялось округление в зависимости от значений точности и масштаба, заданных для столбца базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-180">This means the full precision and scale was sent to SQL Server, at which point SQL Server would round based on the precision and scale of the database column.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-181">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-181">New behavior</span></span>

<span data-ttu-id="d6f2c-182">Теперь EF Core устанавливает значения точности и масштаба для параметров, используя значения, которые заданы в свойствах модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-182">EF Core now sets precision and scale on parameters using the values configured for properties in the EF Core model.</span></span> <span data-ttu-id="d6f2c-183">Таким образом, округление выполняется в SqlClient.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-183">This means rounding now happens in SqlClient.</span></span> <span data-ttu-id="d6f2c-184">Следовательно, если заданные значения точности и масштаба не совпадают с аналогичными в базе данных, округление может выполняться иначе.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-184">Consequentially, if the configured precision and scale do not match the database precision and scale, then the rounding seen may change.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-185">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-185">Why</span></span>

<span data-ttu-id="d6f2c-186">Для новых возможностей SQL Server, включая функцию Always Encrypted, требуется полностью указывать аспекты параметров.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-186">Newer SQL Server features, including Always Encrypted, require that parameter facets are fully specified.</span></span> <span data-ttu-id="d6f2c-187">Кроме того, в результате внесенных в SqlClient изменений вместо усечения десятичных значений выполняется округление, что согласуется с поведением SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-187">In addition, SqlClient made a change to round instead of truncate decimal values, thereby matching the SQL Server behavior.</span></span> <span data-ttu-id="d6f2c-188">Благодаря этому EF Core может задавать эти аспекты для корректной настройки десятичных значений без изменения поведения.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-188">This made it possible for EF Core to set these facets without changing the behavior for correctly configured decimals.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-189">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-189">Mitigations</span></span>

<span data-ttu-id="d6f2c-190">Сопоставьте свойства десятичного значения с использованием имени типа, которое включает в себя значения точности и масштаба.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-190">Map your decimal properties using a type name that includes precision and scale.</span></span> <span data-ttu-id="d6f2c-191">Пример:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-191">For example:</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }

    [Column(TypeName = "decimal(16, 5")]
    public decimal Score { get; set; }
}
```

<span data-ttu-id="d6f2c-192">Также можно использовать `HasPrecision` в API построения модели.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-192">Or use `HasPrecision` in the model building APIs.</span></span> <span data-ttu-id="d6f2c-193">Пример:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-193">For example:</span></span>

```csharp
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>().Property(e => e.Score).HasPrecision(16, 5);
    }
```

<a name="required-dependent"></a>

### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="d6f2c-194">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="d6f2c-194">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="d6f2c-195">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="d6f2c-195">Tracking Issue #17286</span></span>](https://github.com/dotnet/efcore/issues/17286)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-196">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-196">Old behavior</span></span>

<span data-ttu-id="d6f2c-197">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-197">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="d6f2c-198">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-198">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-199">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-199">New behavior</span></span>

<span data-ttu-id="d6f2c-200">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-200">With the added support for required dependents, it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="d6f2c-201">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-201">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

#### <a name="why"></a><span data-ttu-id="d6f2c-202">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-202">Why</span></span>

<span data-ttu-id="d6f2c-203">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-203">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-204">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-204">Mitigations</span></span>

<span data-ttu-id="d6f2c-205">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-205">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a><span data-ttu-id="d6f2c-206">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="d6f2c-206">Defining query is replaced with provider-specific methods</span></span>

[<span data-ttu-id="d6f2c-207">Отслеживание вопроса № 18903</span><span class="sxs-lookup"><span data-stu-id="d6f2c-207">Tracking Issue #18903</span></span>](https://github.com/dotnet/efcore/issues/18903)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-208">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-208">Old behavior</span></span>

<span data-ttu-id="d6f2c-209">Типы сущностей сопоставлялись с определяющими запросами на уровне ядра.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-209">Entity types were mapped to defining queries at the Core level.</span></span> <span data-ttu-id="d6f2c-210">Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-210">Anytime the entity type was used in the query root of the entity type was replaced by the defining query for any provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-211">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-211">New behavior</span></span>

<span data-ttu-id="d6f2c-212">Интерфейсы API для определяющего запроса стали нерекомендуемыми.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-212">APIs for defining query are deprecated.</span></span> <span data-ttu-id="d6f2c-213">Появились новые интерфейсы API для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-213">New provider-specific APIs were introduced.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-214">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-214">Why</span></span>

<span data-ttu-id="d6f2c-215">Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-215">While defining queries were implemented as replacement query whenever query root is used in the query, it had a few issues:</span></span>

- <span data-ttu-id="d6f2c-216">Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-216">If defining query is projecting entity type using `new { ... }` in `Select` method, then identifying that as an entity required additional work and made it inconsistent with how EF Core treats nominal types in the query.</span></span>
- <span data-ttu-id="d6f2c-217">Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-217">For relational providers `FromSql` is still needed to pass the SQL string in LINQ expression form.</span></span>

<span data-ttu-id="d6f2c-218">Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-218">Initially defining queries were introduced as client-side views to be used with In-Memory provider for keyless entities (similar to database views in relational databases).</span></span> <span data-ttu-id="d6f2c-219">Такое определение упрощает тестирование приложения с базой данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-219">Such definition makes it easy to test application against in-memory database.</span></span> <span data-ttu-id="d6f2c-220">Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-220">Afterwards they became broadly applicable, which was useful but brought inconsistent and hard to understand behavior.</span></span> <span data-ttu-id="d6f2c-221">Поэтому мы решили упростить принцип их работы.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-221">So we decided to simplify the concept.</span></span> <span data-ttu-id="d6f2c-222">Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-222">We made LINQ based defining query exclusive to In-Memory provider and treat them differently.</span></span> <span data-ttu-id="d6f2c-223">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-223">For more information, [see this issue](https://github.com/dotnet/efcore/issues/20023).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-224">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-224">Mitigations</span></span>

<span data-ttu-id="d6f2c-225">Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-225">For relational providers, use `ToSqlQuery` method in `OnModelCreating` and pass in a SQL string to use for the entity type.</span></span>
<span data-ttu-id="d6f2c-226">Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-226">For the In-Memory provider, use `ToInMemoryQuery` method in `OnModelCreating` and pass in a LINQ query to use for the entity type.</span></span>

<a name="nonnullreferences"></a>

### <a name="non-null-reference-navigations-are-not-overwritten-by-queries"></a><span data-ttu-id="d6f2c-227">Навигации по ссылке, которая не является пустой, не переопределяются запросами</span><span class="sxs-lookup"><span data-stu-id="d6f2c-227">Non-null reference navigations are not overwritten by queries</span></span>

[<span data-ttu-id="d6f2c-228">Отслеживание проблемы № 2693</span><span class="sxs-lookup"><span data-stu-id="d6f2c-228">Tracking Issue #2693</span></span>](https://github.com/dotnet/EntityFramework.Docs/issues/2693)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-229">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-229">Old behavior</span></span>

<span data-ttu-id="d6f2c-230">В EF Core 3.1 навигации по ссылкам, которые изначально инициализированы значениями, отличными от NULL, иногда перезаписывались экземплярами сущностей из базы данных, независимо от того, совпадали ли значения ключей.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-230">In EF Core 3.1, reference navigations eagerly initialized to non-null values would sometimes be overwritten by entity instances from the database, regardless of whether or not key values matched.</span></span> <span data-ttu-id="d6f2c-231">Однако в других случаях в EF Core 3.1 выполнялось обратное действие и оставлялось существующее значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-231">However, in other cases, EF Core 3.1 would do the opposite and leave the existing non-null value.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-232">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-232">New behavior</span></span>

<span data-ttu-id="d6f2c-233">Начиная с версии EF Core 5.0, навигации по пустой ссылке, которая не является пустой, никогда не перезаписываются экземплярами, возвращаемыми запросом.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-233">Starting with EF Core 5.0, non-null reference navigations are never overwritten by instances returned from a query.</span></span>

<span data-ttu-id="d6f2c-234">Обратите внимание, что безотложная инициализация навигации по _коллекции_ для пустой коллекции по-прежнему поддерживается.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-234">Note that eager initialization of a _collection_ navigation to an empty collection is still supported.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-235">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-235">Why</span></span>

<span data-ttu-id="d6f2c-236">Инициализация свойства навигации по ссылкам для "пустого" экземпляра сущности приводит к неоднозначному состоянию.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-236">Initialization of a reference navigation property to an "empty" entity instance results in an ambiguous state.</span></span> <span data-ttu-id="d6f2c-237">Пример:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-237">For example:</span></span>

```csharp
public class Blog
{
     public int Id { get; set; }
     public Author Author { get; set; ) = new Author();
}
```

<span data-ttu-id="d6f2c-238">Обычно запрос блогов и авторов сначала создает экземпляры `Blog`, а затем задает соответствующие экземпляры `Author` на основе данных, возвращаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-238">Normally a query for Blogs and Authors will first create `Blog` instances and then set the appropriate `Author` instances based on the data returned from the database.</span></span> <span data-ttu-id="d6f2c-239">Однако в этом случае каждое свойство `Blog.Author` будет уже инициализировано для пустого свойства `Author`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-239">However, in this case every `Blog.Author` property is already initialized to an empty `Author`.</span></span> <span data-ttu-id="d6f2c-240">Кроме EF Core, не существует способа выяснить, является ли этот экземпляр "пустым".</span><span class="sxs-lookup"><span data-stu-id="d6f2c-240">Except EF Core has no way to know that this instance is "empty".</span></span> <span data-ttu-id="d6f2c-241">Таким образом, перезапись этого экземпляра может без уведомления привести к выводу допустимого `Author`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-241">So overwriting this instance could potentially silently throw away a valid `Author`.</span></span> <span data-ttu-id="d6f2c-242">Таким образом, EF Core 5.0 теперь не перезаписывает уже инициализированную навигацию.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-242">Therefore, EF Core 5.0 now consistently does not overwrite a navigation that is already initialized.</span></span>

<span data-ttu-id="d6f2c-243">Это новое поведение также соответствует обычному поведению EF6, хотя при исследовании мы также обнаружили некоторые случаи несогласованности в EF6.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-243">This new behavior also aligns with the behavior of EF6 in most cases, although upon investigation we also found some cases of inconsistency in EF6.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-244">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-244">Mitigations</span></span>

<span data-ttu-id="d6f2c-245">В случае обнаружения такого прерывания исправление будет способствовать остановке активной инициализации свойств навигации по ссылкам.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-245">If this break is encountered, then the fix is to stop eagerly initializing reference navigation properties.</span></span>

<a name="toview"></a>

### <a name="toview-is-treated-differently-by-migrations"></a><span data-ttu-id="d6f2c-246">ToView() по-разному обрабатывается миграциями</span><span class="sxs-lookup"><span data-stu-id="d6f2c-246">ToView() is treated differently by migrations</span></span>

[<span data-ttu-id="d6f2c-247">Отслеживание проблемы № 2725</span><span class="sxs-lookup"><span data-stu-id="d6f2c-247">Tracking Issue #2725</span></span>](https://github.com/dotnet/efcore/issues/2725)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-248">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-248">Old behavior</span></span>

<span data-ttu-id="d6f2c-249">Вызов `ToView(string)` заставляет миграции игнорировать тип сущности в дополнение к сопоставлению его с представлением.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-249">Calling `ToView(string)` made the migrations ignore the entity type in addition to mapping it to a view.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-250">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-250">New behavior</span></span>

<span data-ttu-id="d6f2c-251">Теперь `ToView(string)` помечает тип сущности как не сопоставленный с таблицей в дополнение к сопоставлению его с представлением.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-251">Now `ToView(string)` marks the entity type as not mapped to a table in addition to mapping it to a view.</span></span> <span data-ttu-id="d6f2c-252">Это приводит к тому, что при первой миграции после обновления до EF Core 5 предпринимается попытка удаления таблицы по умолчанию для этого типа сущности, так как он больше не игнорируется.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-252">This results in the first migration after upgrading to EF Core 5 to try to drop the default table for this entity type as it's not longer ignored.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-253">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-253">Why</span></span>

<span data-ttu-id="d6f2c-254">EF Core теперь позволяет сопоставлять тип сущности и с таблицей, и с представлением одновременно, поэтому `ToView` больше нельзя использовать для указания, что тип сущности следует игнорировать при миграции.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-254">EF Core now allows an entity type to be mapped to both a table and a view simultaneously, so `ToView` is no longer a valid indicator that it should be ignored by migrations.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-255">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-255">Mitigations</span></span>

<span data-ttu-id="d6f2c-256">Используйте следующий код, чтобы пометить сопоставленную таблицу как исключенную из миграции:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-256">Use the following code to mark the mapped table as excluded from migrations:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<User>().ToTable("UserView", t => t.ExcludeFromMigrations());
}
```

<a name="totable"></a>

### <a name="totablenull-marks-the-entity-type-as-not-mapped-to-a-table"></a><span data-ttu-id="d6f2c-257">ToTable(null) помечает тип сущности как не сопоставленный с таблицей</span><span class="sxs-lookup"><span data-stu-id="d6f2c-257">ToTable(null) marks the entity type as not mapped to a table</span></span>

[<span data-ttu-id="d6f2c-258">Отслеживание проблемы № 21172</span><span class="sxs-lookup"><span data-stu-id="d6f2c-258">Tracking Issue #21172</span></span>](https://github.com/dotnet/efcore/issues/21172)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-259">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-259">Old behavior</span></span>

<span data-ttu-id="d6f2c-260">`ToTable(null)` сбрасывает имя таблицы в значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-260">`ToTable(null)` would reset the table name to the default.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-261">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-261">New behavior</span></span>

<span data-ttu-id="d6f2c-262">`ToTable(null)` теперь помечает тип сущности как не сопоставленный с какой-либо таблицей.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-262">`ToTable(null)` now marks the entity type as not mapped to any table.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-263">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-263">Why</span></span>

<span data-ttu-id="d6f2c-264">EF Core теперь позволяет сопоставлять тип сущности и с таблицей, и с представлением одновременно, поэтому `ToTable(null)` используется для указания, что тип сущности не сопоставлен ни с какой таблицей.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-264">EF Core now allows an entity type to be mapped to both a table and a view simultaneously, so `ToTable(null)` is used to indicate that it isn't mapped to any table.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-265">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-265">Mitigations</span></span>

<span data-ttu-id="d6f2c-266">Используйте следующий код, чтобы сбрасывать имя таблицы в значение по умолчанию, если отсутствует сопоставление с представлением или DbFunction:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-266">Use the following code to reset the table name to the default if it's not mapped to a view or a DbFunction:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<User>().Metadata.RemoveAnnotation(RelationalAnnotationNames.TableName);
}
```

## <a name="low-impact-changes"></a><span data-ttu-id="d6f2c-267">Изменения низкой степени влияния</span><span class="sxs-lookup"><span data-stu-id="d6f2c-267">Low-impact changes</span></span>

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="d6f2c-268">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-268">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="d6f2c-269">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="d6f2c-269">Tracking Issue #14257</span></span>](https://github.com/dotnet/efcore/issues/14257)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-270">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-270">Old behavior</span></span>

<span data-ttu-id="d6f2c-271">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-271">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="d6f2c-272">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-272">However, it only ever affected database creation.</span></span> <span data-ttu-id="d6f2c-273">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-273">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="d6f2c-274">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/dotnet/efcore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-274">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/dotnet/efcore/issues/14257)).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-275">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-275">New behavior</span></span>

<span data-ttu-id="d6f2c-276">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-276">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="d6f2c-277">Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-277">This API matches more closely to the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-278">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-278">Why</span></span>

<span data-ttu-id="d6f2c-279">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-279">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-280">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-280">Mitigations</span></span>

<span data-ttu-id="d6f2c-281">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-281">Use `HasColumnType` to specify the dimension:</span></span>

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

<a name="cosmos-partition-key"></a>

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a><span data-ttu-id="d6f2c-282">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="d6f2c-282">Cosmos: Partition key is now added to the primary key</span></span>

[<span data-ttu-id="d6f2c-283">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="d6f2c-283">Tracking Issue #15289</span></span>](https://github.com/dotnet/efcore/issues/15289)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-284">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-284">Old behavior</span></span>

<span data-ttu-id="d6f2c-285">Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-285">The partition key property was only added to the alternate key that includes `id`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-286">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-286">New behavior</span></span>

<span data-ttu-id="d6f2c-287">В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-287">The partition key property is now also added to the primary key by convention.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-288">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-288">Why</span></span>

<span data-ttu-id="d6f2c-289">Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-289">This change makes the model better aligned with Azure Cosmos DB semantics and improves the performance of `Find` and some queries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-290">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-290">Mitigations</span></span>

<span data-ttu-id="d6f2c-291">Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-291">To prevent the partition key property to be added to the primary key, configure it in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a><span data-ttu-id="d6f2c-292">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="d6f2c-292">Cosmos: `id` property renamed to `__id`</span></span>

[<span data-ttu-id="d6f2c-293">Отслеживание вопроса № 17751</span><span class="sxs-lookup"><span data-stu-id="d6f2c-293">Tracking Issue #17751</span></span>](https://github.com/dotnet/efcore/issues/17751)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-294">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-294">Old behavior</span></span>

<span data-ttu-id="d6f2c-295">Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-295">The shadow property mapped to the `id` JSON property was also named `id`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-296">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-296">New behavior</span></span>

<span data-ttu-id="d6f2c-297">Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-297">The shadow property created by convention is now named `__id`.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-298">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-298">Why</span></span>

<span data-ttu-id="d6f2c-299">Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-299">This change makes it less likely that the `id` property clashes with an existing property on the entity type.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-300">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-300">Mitigations</span></span>

<span data-ttu-id="d6f2c-301">Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-301">To go back to the 3.x behavior, configure the `id` property in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a><span data-ttu-id="d6f2c-302">Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива</span><span class="sxs-lookup"><span data-stu-id="d6f2c-302">Cosmos: byte[] is now stored as a base64 string instead of a number array</span></span>

[<span data-ttu-id="d6f2c-303">Отслеживание вопроса № 17306</span><span class="sxs-lookup"><span data-stu-id="d6f2c-303">Tracking Issue #17306</span></span>](https://github.com/dotnet/efcore/issues/17306)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-304">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-304">Old behavior</span></span>

<span data-ttu-id="d6f2c-305">Свойства типа byte[] хранились в виде числового массива.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-305">Properties of type byte[] were stored as a number array.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-306">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-306">New behavior</span></span>

<span data-ttu-id="d6f2c-307">Свойства типа byte[] теперь хранятся в виде строки base64.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-307">Properties of type byte[] are now stored as a base64 string.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-308">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-308">Why</span></span>

<span data-ttu-id="d6f2c-309">Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-309">This representation of byte[] aligns better with expectations and is the default behavior of the major JSON serialization libraries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-310">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-310">Mitigations</span></span>

<span data-ttu-id="d6f2c-311">Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-311">Existing data stored as number arrays will still be queried correctly, but currently there isn't a supported way to change back the insert behavior.</span></span> <span data-ttu-id="d6f2c-312">Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/dotnet/efcore/issues/17306)</span><span class="sxs-lookup"><span data-stu-id="d6f2c-312">If this limitation is blocking your scenario, comment on [this issue](https://github.com/dotnet/efcore/issues/17306)</span></span>

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a><span data-ttu-id="d6f2c-313">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="d6f2c-313">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>

[<span data-ttu-id="d6f2c-314">Отслеживание вопроса № 17874</span><span class="sxs-lookup"><span data-stu-id="d6f2c-314">Tracking Issue #17874</span></span>](https://github.com/dotnet/efcore/issues/17874)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-315">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-315">Old behavior</span></span>

<span data-ttu-id="d6f2c-316">Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-316">Previously the extension methods were called `GetPropertyName` and `SetPropertyName`</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-317">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-317">New behavior</span></span>

<span data-ttu-id="d6f2c-318">Старый интерфейс API был удален, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-318">The old API was removed and new methods added: `GetJsonPropertyName`, `SetJsonPropertyName`</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-319">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-319">Why</span></span>

<span data-ttu-id="d6f2c-320">Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-320">This change removes the ambiguity around what these methods are configuring.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-321">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-321">Mitigations</span></span>

<span data-ttu-id="d6f2c-322">Используйте новый API.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-322">Use the new API.</span></span>

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="d6f2c-323">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="d6f2c-323">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>

[<span data-ttu-id="d6f2c-324">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="d6f2c-324">Tracking Issue #15289</span></span>](https://github.com/dotnet/efcore/issues/15289)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-325">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-325">Old behavior</span></span>

<span data-ttu-id="d6f2c-326">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="d6f2c-326">Value generators were only called when the entity state changed to Added.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-327">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-327">New behavior</span></span>

<span data-ttu-id="d6f2c-328">Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-328">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated, or Deleted and the property contains the default values.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-329">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-329">Why</span></span>

<span data-ttu-id="d6f2c-330">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-330">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-331">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-331">Mitigations</span></span>

<span data-ttu-id="d6f2c-332">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-332">To prevent the value generator from being called, assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>

### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="d6f2c-333">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="d6f2c-333">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="d6f2c-334">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="d6f2c-334">Tracking Issue #20305</span></span>](https://github.com/dotnet/efcore/issues/20305)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-335">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-335">Old behavior</span></span>

<span data-ttu-id="d6f2c-336">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-336">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-337">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-337">New behavior</span></span>

<span data-ttu-id="d6f2c-338">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-338">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="d6f2c-339">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-339">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-340">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-340">Why</span></span>

<span data-ttu-id="d6f2c-341">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-341">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="d6f2c-342">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-342">Using it to find differences is faster and more accurate.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-343">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-343">Mitigations</span></span>

<span data-ttu-id="d6f2c-344">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-344">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

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

<span data-ttu-id="d6f2c-345">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-345">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="read-only-discriminators"></a>

### <a name="discriminators-are-read-only"></a><span data-ttu-id="d6f2c-346">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="d6f2c-346">Discriminators are read-only</span></span>

[<span data-ttu-id="d6f2c-347">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="d6f2c-347">Tracking Issue #21154</span></span>](https://github.com/dotnet/efcore/issues/21154)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-348">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-348">Old behavior</span></span>

<span data-ttu-id="d6f2c-349">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="d6f2c-349">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-350">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-350">New behavior</span></span>

<span data-ttu-id="d6f2c-351">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-351">An exception will be throws in the above case.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-352">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-352">Why</span></span>

<span data-ttu-id="d6f2c-353">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-353">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state, which might result in unexpected behavior.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-354">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-354">Mitigations</span></span>

<span data-ttu-id="d6f2c-355">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-355">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges`, the discriminator can be made mutable:</span></span>

```csharp
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```

<a name="no-client-methods"></a>

### <a name="provider-specific-effunctions-methods-throw-for-inmemory-provider"></a><span data-ttu-id="d6f2c-356">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="d6f2c-356">Provider-specific EF.Functions methods throw for InMemory provider</span></span>

[<span data-ttu-id="d6f2c-357">Отслеживание вопроса № 20294</span><span class="sxs-lookup"><span data-stu-id="d6f2c-357">Tracking Issue #20294</span></span>](https://github.com/dotnet/efcore/issues/20294)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-358">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-358">Old behavior</span></span>

<span data-ttu-id="d6f2c-359">Относящиеся к конкретному поставщику методы EF.Functions содержали реализацию для выполнения на стороне клиента, что позволяло им выполняться в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-359">Provider-specific EF.Functions methods contained implementation for client execution, which allowed them to be executed on the InMemory provider.</span></span> <span data-ttu-id="d6f2c-360">Например, `EF.Functions.DateDiffDay` — это относящийся к SQL Server метод, который работал в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-360">For example, `EF.Functions.DateDiffDay` is a Sql Server specific method, which worked on InMemory provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-361">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-361">New behavior</span></span>

<span data-ttu-id="d6f2c-362">Методы, относящиеся к конкретному поставщику, были изменены так, чтобы в теле метода создавалось исключение, блокирующее вычисление метода на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-362">Provider-specific methods have been updated to throw an exception in their method body to block evaluating them on client side.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-363">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-363">Why</span></span>

<span data-ttu-id="d6f2c-364">Методы, относящиеся к конкретному поставщику, сопоставляются с функцией базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-364">Provider-specific methods map to a database function.</span></span> <span data-ttu-id="d6f2c-365">Вычисление, выполняемое сопоставленной функцией базы данных, не всегда может реплицироваться на стороне клиента в LINQ.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-365">The computation done by the mapped database function can't always be replicated on the client side in LINQ.</span></span> <span data-ttu-id="d6f2c-366">Поэтому результаты, полученные при выполнении одного и того же метода на сервере и в клиенте, могут различаться.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-366">It may cause the result from the server to differ when executing the same method on client.</span></span> <span data-ttu-id="d6f2c-367">Так как эти методы используются в LINQ для преобразования в определенные функции базы данных, их не нужно вычислять на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-367">Since these methods are used in LINQ to translate to specific database functions, they don't need to be evaluated on client side.</span></span> <span data-ttu-id="d6f2c-368">Так как поставщик InMemory — это другая *база данных*, эти методы недоступны для него.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-368">As InMemory provider is a different *database*, these methods aren't available for this provider.</span></span> <span data-ttu-id="d6f2c-369">При попытке выполнить их для поставщика InMemory или любого другого поставщика, который не преобразует эти методы, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-369">Trying to execute them for InMemory provider, or any other provider that doesn't translate these methods, throws an exception.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-370">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-370">Mitigations</span></span>

<span data-ttu-id="d6f2c-371">Так как точно воссоздать поведение функций базы данных невозможно, содержащие их запросы следует тестировать на основе базы данных того же типа, что и рабочая база данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-371">Since there's no way to mimic behavior of database functions accurately, you should test the queries containing them against same kind of database as in production.</span></span>

<a name="index-obsolete"></a>

### <a name="indexbuilderhasname-is-now-obsolete"></a><span data-ttu-id="d6f2c-372">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="d6f2c-372">IndexBuilder.HasName is now obsolete</span></span>

[<span data-ttu-id="d6f2c-373">Отслеживание проблемы № 21089</span><span class="sxs-lookup"><span data-stu-id="d6f2c-373">Tracking Issue #21089</span></span>](https://github.com/dotnet/efcore/issues/21089)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-374">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-374">Old behavior</span></span>

<span data-ttu-id="d6f2c-375">Ранее допускалось определение только одного индекса для конкретного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-375">Previously, only one index could be defined over a given set of properties.</span></span> <span data-ttu-id="d6f2c-376">Имя для базы данных индекса настраивалось с использованием IndexBuilder.HasName.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-376">The database name of an index was configured using IndexBuilder.HasName.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-377">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-377">New behavior</span></span>

<span data-ttu-id="d6f2c-378">Теперь допускается несколько индексов для одного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-378">Multiple indexes are now allowed on the same set or properties.</span></span> <span data-ttu-id="d6f2c-379">Эти индексы отличаются именами в пределах модели.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-379">These indexes are now distinguished by a name in the model.</span></span> <span data-ttu-id="d6f2c-380">По соглашению имя модели используется в качестве имени базы данных, но его также можно указать независимо с использованием HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-380">By convention, the model name is used as the database name; however it can also be configured independently using HasDatabaseName.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-381">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-381">Why</span></span>

<span data-ttu-id="d6f2c-382">В будущем мы хотим включить для набора свойств параллельные индексы с сортировкой по возрастанию и убыванию, а также индексы с разными параметрами сортировки.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-382">In the future, we'd like to enable both ascending and descending indexes or indexes with different collations on the same set of properties.</span></span> <span data-ttu-id="d6f2c-383">Это изменение продвигает нас в этом направлении.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-383">This change moves us another step in that direction.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-384">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-384">Mitigations</span></span>

<span data-ttu-id="d6f2c-385">Любой код, который ранее вызывал IndexBuilder.HasName, необходимо изменить для вызова HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-385">Any code that was previously calling IndexBuilder.HasName should be updated to call HasDatabaseName instead.</span></span>

<span data-ttu-id="d6f2c-386">Если в проекте есть миграции, созданные до версии EF Core 2.0.0, вы можете игнорировать предупреждения для этих файлов, подавив их с помощью `#pragma warning disable 612, 618`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-386">If your project includes migrations generated prior to EF Core version 2.0.0, you can safely ignore the warning in those files and suppress it by adding `#pragma warning disable 612, 618`.</span></span>

<a name="pluralizer"></a>

### <a name="a-pluralizer-is-now-included-for-scaffolding-reverse-engineered-models"></a><span data-ttu-id="d6f2c-387">Добавлено средство преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="d6f2c-387">A pluralizer is now included for scaffolding reverse engineered models</span></span>

[<span data-ttu-id="d6f2c-388">Отслеживание проблемы № 11160</span><span class="sxs-lookup"><span data-stu-id="d6f2c-388">Tracking Issue #11160</span></span>](https://github.com/dotnet/efcore/issues/11160)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-389">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-389">Old behavior</span></span>

<span data-ttu-id="d6f2c-390">Ранее нужно было устанавливать отдельный пакет, чтобы преобразовывать имена DbSet и коллекций во множественное число и имена таблиц в единственное число при создании шаблонов DbContext и типов сущностей путем реконструирования из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-390">Previously, you had to install a separate pluralizer package in order to pluralize DbSet and collection navigation names and singularize table names when scaffolding a DbContext and entity types by reverse engineering a database schema.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-391">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-391">New behavior</span></span>

<span data-ttu-id="d6f2c-392">Теперь EF Core содержит собственное средство для преобразования во множественное число на основе библиотеки [Humanizer](https://github.com/Humanizr/Humanizer).</span><span class="sxs-lookup"><span data-stu-id="d6f2c-392">EF Core now includes a pluralizer that uses the [Humanizer](https://github.com/Humanizr/Humanizer) library.</span></span> <span data-ttu-id="d6f2c-393">Именно эту библиотеку использует Visual Studio, рекомендуя имена переменных.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-393">This is the same library Visual Studio uses to recommend variable names.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-394">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-394">Why</span></span>

<span data-ttu-id="d6f2c-395">Использование форм множественного числа для свойств коллекций и единственного числа для типов и ссылочных свойств считается идиоматичным в .NET.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-395">Using plural forms of words for collection properties and singular forms for types and reference properties is idiomatic in .NET.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-396">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-396">Mitigations</span></span>

<span data-ttu-id="d6f2c-397">Чтобы отключить средство для преобразования во множественное число, укажите параметр `--no-pluralize` для `dotnet ef dbcontext scaffold` или `-NoPluralize` для `Scaffold-DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-397">To disable the pluralizer, use the `--no-pluralize` option on `dotnet ef dbcontext scaffold` or the `-NoPluralize` switch on `Scaffold-DbContext`.</span></span>

<a name="inavigationbase"></a>

### <a name="inavigationbase-replaces-inavigation-in-some-apis-to-support-skip-navigations"></a><span data-ttu-id="d6f2c-398">INavigationBase заменяет INavigation в некоторых API, чтобы поддерживать пропуск навигации</span><span class="sxs-lookup"><span data-stu-id="d6f2c-398">INavigationBase replaces INavigation in some APIs to support skip navigations</span></span>

[<span data-ttu-id="d6f2c-399">Отслеживание проблемы № 2568</span><span class="sxs-lookup"><span data-stu-id="d6f2c-399">Tracking Issue #2568</span></span>](https://github.com/dotnet/EntityFramework.Docs/issues/2568)

#### <a name="old-behavior"></a><span data-ttu-id="d6f2c-400">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-400">Old behavior</span></span>

<span data-ttu-id="d6f2c-401">В версиях EF Core, предшествующих версии 5.0, поддерживалась только одна форма свойства навигации, представленная интерфейсом `INavigation`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-401">EF Core prior to 5.0 supported only one form of navigation property, represented by the `INavigation` interface.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d6f2c-402">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="d6f2c-402">New behavior</span></span>

<span data-ttu-id="d6f2c-403">EF Core 5.0 представляет связи "многие ко многим", которые используют "пропуск навигации".</span><span class="sxs-lookup"><span data-stu-id="d6f2c-403">EF Core 5.0 introduces many-to-many relationships which use "skip navigations".</span></span> <span data-ttu-id="d6f2c-404">Они представлены интерфейсом `ISkipNavigation`, а большинство функциональных возможностей `INavigation` переданы в общий базовый интерфейс: `INavigationBase`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-404">These are represented by the `ISkipNavigation` interface, and most of the functionality of `INavigation` has been pushed down to a common base interface: `INavigationBase`.</span></span>

#### <a name="why"></a><span data-ttu-id="d6f2c-405">Почему</span><span class="sxs-lookup"><span data-stu-id="d6f2c-405">Why</span></span>

<span data-ttu-id="d6f2c-406">Большая часть функций между обычной навигацией и пропуском навигации одинаковы.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-406">Most of the functionality between normal and skip navigations is the same.</span></span> <span data-ttu-id="d6f2c-407">Тем не менее пропуск навигации имеет другую связь с внешними ключами по сравнению с обычной навигацией, поскольку вовлеченные внешние ключи находятся не на обоих концах связи, а скорее в объекте соединения.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-407">However, skip navigations have a different relationship to foreign keys than normal navigations, since the FKs involved are not directly on either end of the relationship, but rather in the join entity.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="d6f2c-408">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="d6f2c-408">Mitigations</span></span>

<span data-ttu-id="d6f2c-409">Во многих случаях приложения могут переключиться на использование нового базового интерфейса, не нуждаясь в дополнительных изменениях.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-409">In many cases applications can switch to using the new base interface with no other changes.</span></span> <span data-ttu-id="d6f2c-410">Однако если навигация используется для доступа к свойствам внешнего ключа, код приложения нужно либо ограничить только обычными переходами, либо обновить, чтобы он мог выполнять соответствующие действия как для обычной, так и для пропускной навигации.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-410">However, in cases where the navigation is used to access foreign key properties, application code should either be constrained to only normal navigations, or updated to do the appropriate thing for both normal and skip navigations.</span></span>

<a name="collection-distinct-groupby"></a>

### <a name="some-queries-with-correlated-collection-that-also-use-distinct-or-groupby-are-no-longer-supported"></a><span data-ttu-id="d6f2c-411">Некоторые запросы с помощью коррелированной коллекции, которые также используют `Distinct` или `GroupBy`, больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-411">Some queries with correlated collection that also use `Distinct` or `GroupBy` are no longer supported</span></span>

[<span data-ttu-id="d6f2c-412">Отслеживание проблемы № 15873</span><span class="sxs-lookup"><span data-stu-id="d6f2c-412">Tracking Issue #15873</span></span>](https://github.com/dotnet/efcore/issues/15873)

<span data-ttu-id="d6f2c-413">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-413">**Old behavior**</span></span>

<span data-ttu-id="d6f2c-414">Ранее мы позволяли выполнять запросы, включающие коррелированные коллекции с последующим свойством `GroupBy` и некоторые запросы с использованием `Distinct`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-414">Previously, queries involving correlated collections followed by `GroupBy`, as well as some queries using `Distinct` we allowed to execute.</span></span>

<span data-ttu-id="d6f2c-415">Пример GroupBy:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-415">GroupBy example:</span></span>

```csharp
context.Parents
    .Select(p => p.Children
        .GroupBy(c => c.School)
        .Select(g => g.Key))
```

<span data-ttu-id="d6f2c-416">Пример `Distinct`, в частности запросы `Distinct`, в которых внутренняя проекция коллекции не содержит первичный ключ:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-416">`Distinct` example - specifically `Distinct` queries where inner collection projection doesn't contain the primary key:</span></span>

```csharp
context.Parents
    .Select(p => p.Children
        .Select(c => c.School)
        .Distinct())
```

<span data-ttu-id="d6f2c-417">Эти запросы могли возвращать неверные результаты, если внутренняя коллекция содержала дубликаты, однако работала должным образом, если все элементы во внутренней коллекции являлись уникальными.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-417">These queries could return incorrect results if the inner collection contained any duplicates, but worked correctly if all the elements in the inner collection were unique.</span></span>

<span data-ttu-id="d6f2c-418">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-418">**New behavior**</span></span>

<span data-ttu-id="d6f2c-419">Эти запросы больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-419">These queries are no longer supported.</span></span> <span data-ttu-id="d6f2c-420">Создается исключение, указывающее, что у нас недостаточно сведений для правильного создания результатов.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-420">Exception is thrown indicating that we don't have enough information to correctly build the results.</span></span>

<span data-ttu-id="d6f2c-421">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-421">**Why**</span></span>

<span data-ttu-id="d6f2c-422">В сценариях с коррелированными коллекциями, чтобы назначить сущность коллекции правильному родительскому объекту, нужно знать первичный ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-422">For correlated collection scenarios we need to know entity's primary key in order to assign collection entities to the correct parent.</span></span> <span data-ttu-id="d6f2c-423">Если внутренняя коллекция не использует `GroupBy` или `Distinct`, отсутствующий первичный ключ можно просто добавить в проекцию.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-423">When inner collection doesn't use `GroupBy` or `Distinct`, the missing primary key can simply be added to the projection.</span></span> <span data-ttu-id="d6f2c-424">Однако при использовании `GroupBy` и `Distinct` это не удастся сделать, так как такое действие приведет к изменению результата операции `GroupBy` или `Distinct`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-424">However in case of `GroupBy` and `Distinct` it can't be done because it would change the result of `GroupBy` or `Distinct` operation.</span></span>

<span data-ttu-id="d6f2c-425">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-425">**Mitigations**</span></span>

<span data-ttu-id="d6f2c-426">Перепишите запрос, чтобы не использовать операции `GroupBy` или `Distinct` во внутренней коллекции, и выполните эти операции на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-426">Rewrite the query to not use `GroupBy` or `Distinct` operations on the inner collection, and perform these operations on the client instead.</span></span>

```csharp
context.Parents
    .Select(p => p.Children.Select(c => c.School))
    .ToList()
    .Select(x => x.GroupBy(c => c).Select(g => g.Key))
```

```csharp
context.Parents
    .Select(p => p.Children.Select(c => c.School))
    .ToList()
    .Select(x => x.Distinct())
```

<a name="queryable-projection"></a>

### <a name="using-a-collection-of-queryable-type-in-projection-is-not-supported"></a><span data-ttu-id="d6f2c-427">Использование коллекции запрашиваемого типа в проекции не поддерживается</span><span class="sxs-lookup"><span data-stu-id="d6f2c-427">Using a collection of Queryable type in projection is not supported</span></span>

[<span data-ttu-id="d6f2c-428">Отслеживание проблемы № 16314</span><span class="sxs-lookup"><span data-stu-id="d6f2c-428">Tracking Issue #16314</span></span>](https://github.com/dotnet/efcore/issues/16314)

<span data-ttu-id="d6f2c-429">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-429">**Old behavior**</span></span>

<span data-ttu-id="d6f2c-430">Ранее в отдельных случаях коллекцию запрашиваемых типов можно было использовать в проекции, например как аргумент для конструктора `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-430">Previously, it was possible to use collection of a Queryable type inside the projection in some cases, for example as an argument to a `List<T>` constructor:</span></span>

```csharp
context.Blogs
    .Select(b => new List<Post>(context.Posts.Where(p => p.BlogId == b.Id)))
```

<span data-ttu-id="d6f2c-431">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-431">**New behavior**</span></span>

<span data-ttu-id="d6f2c-432">Эти запросы больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-432">These queries are no longer supported.</span></span> <span data-ttu-id="d6f2c-433">Создается исключение с указанием на невозможность создания объекта запрашиваемого типа и предложением того, как это можно исправить.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-433">Exception is thrown indicating that we can't create an object of Queryable type and suggesting how this could be fixed.</span></span>

<span data-ttu-id="d6f2c-434">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-434">**Why**</span></span>

<span data-ttu-id="d6f2c-435">Мы не можем реализовать объект запрашиваемого типа, поэтому такие объекты автоматически создавались с помощью типа `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-435">We can't materialize an object of a Queryable type, so they would automatically be created using `List<T>` type instead.</span></span> <span data-ttu-id="d6f2c-436">Это часто приводило к выводу исключения из-за несоответствия типов, которое было непонятным и могло быть неожиданным для некоторых пользователей.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-436">This would often cause an exception due to type mismatch which was not very clear and could be surprising to some users.</span></span> <span data-ttu-id="d6f2c-437">Мы решили распознать шаблон и выдать более осмысленное исключение.</span><span class="sxs-lookup"><span data-stu-id="d6f2c-437">We decided to recognize the pattern and throw a more meaningful exception.</span></span>

<span data-ttu-id="d6f2c-438">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d6f2c-438">**Mitigations**</span></span>

<span data-ttu-id="d6f2c-439">Добавьте вызов `ToList()` после запрашиваемого объекта в проекции:</span><span class="sxs-lookup"><span data-stu-id="d6f2c-439">Add `ToList()` call after the Queryable object in the projection:</span></span>

```csharp
context.Blogs.Select(b => context.Posts.Where(p => p.BlogId == b.Id).ToList())
```
