---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 11/07/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: 7a13c9a6f6bd299991c379ec490480e1fbb4ba46
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635475"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="e0e29-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="e0e29-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="e0e29-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="e0e29-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="e0e29-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="e0e29-105">Summary</span></span>

| <span data-ttu-id="e0e29-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="e0e29-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="e0e29-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="e0e29-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="e0e29-108">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="e0e29-108">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="e0e29-109">Средний</span><span class="sxs-lookup"><span data-stu-id="e0e29-109">Medium</span></span>     |
| [<span data-ttu-id="e0e29-110">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="e0e29-110">Defining query is replaced with provider-specific methods</span></span>](#defining-query)                                                          | <span data-ttu-id="e0e29-111">Средний</span><span class="sxs-lookup"><span data-stu-id="e0e29-111">Medium</span></span>     |
| [<span data-ttu-id="e0e29-112">Навигации по ссылке, которая не является пустой, не переопределяются запросами</span><span class="sxs-lookup"><span data-stu-id="e0e29-112">Non-null reference navigations are not overwritten by queries</span></span>](#nonnullreferences)                                                   | <span data-ttu-id="e0e29-113">Средний</span><span class="sxs-lookup"><span data-stu-id="e0e29-113">Medium</span></span>     |
| [<span data-ttu-id="e0e29-114">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="e0e29-114">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="e0e29-115">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-115">Low</span></span>        |
| [<span data-ttu-id="e0e29-116">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="e0e29-116">Cosmos: Partition key is now added to the primary key</span></span>](#cosmos-partition-key)                                                        | <span data-ttu-id="e0e29-117">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-117">Low</span></span>        |
| [<span data-ttu-id="e0e29-118">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="e0e29-118">Cosmos: `id` property renamed to `__id`</span></span>](#cosmos-id)                                                                                 | <span data-ttu-id="e0e29-119">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-119">Low</span></span>        |
| <span data-ttu-id="e0e29-120">[Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)</span><span class="sxs-lookup"><span data-stu-id="e0e29-120">[Cosmos: byte[] is now stored as a base64 string instead of a number array](#cosmos-byte)</span></span>                                             | <span data-ttu-id="e0e29-121">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-121">Low</span></span>        |
| [<span data-ttu-id="e0e29-122">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="e0e29-122">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>](#cosmos-metadata)                                                          | <span data-ttu-id="e0e29-123">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-123">Low</span></span>        |
| [<span data-ttu-id="e0e29-124">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="e0e29-124">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>](#non-added-generation) | <span data-ttu-id="e0e29-125">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-125">Low</span></span>        |
| [<span data-ttu-id="e0e29-126">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="e0e29-126">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="e0e29-127">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-127">Low</span></span>        |
| [<span data-ttu-id="e0e29-128">ToView() по-разному обрабатывается миграциями</span><span class="sxs-lookup"><span data-stu-id="e0e29-128">ToView() is treated differently by migrations</span></span>](#toview)                                                                              | <span data-ttu-id="e0e29-129">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-129">Low</span></span>        |
| [<span data-ttu-id="e0e29-130">ToTable(null) помечает тип сущности как не сопоставленный с таблицей</span><span class="sxs-lookup"><span data-stu-id="e0e29-130">ToTable(null) marks the entity type as not mapped to a table</span></span>](#totable)                                                              | <span data-ttu-id="e0e29-131">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-131">Low</span></span>        |
| [<span data-ttu-id="e0e29-132">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="e0e29-132">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="e0e29-133">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-133">Low</span></span>        |
| [<span data-ttu-id="e0e29-134">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="e0e29-134">Provider-specific EF.Functions methods throw for InMemory provider</span></span>](#no-client-methods)                                              | <span data-ttu-id="e0e29-135">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-135">Low</span></span>        |
| [<span data-ttu-id="e0e29-136">IProperty.GetColumnName() считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="e0e29-136">IProperty.GetColumnName() is now obsolete</span></span>](#getcolumnname-obsolete)                                                                  | <span data-ttu-id="e0e29-137">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-137">Low</span></span>        |
| [<span data-ttu-id="e0e29-138">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="e0e29-138">IndexBuilder.HasName is now obsolete</span></span>](#index-obsolete)                                                                               | <span data-ttu-id="e0e29-139">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-139">Low</span></span>        |
| [<span data-ttu-id="e0e29-140">Добавлено средство преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="e0e29-140">A pluralizer is now included for scaffolding reverse engineered models</span></span>](#pluralizer)                                                 | <span data-ttu-id="e0e29-141">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-141">Low</span></span>        |
| [<span data-ttu-id="e0e29-142">INavigationBase заменяет INavigation в некоторых API, чтобы поддерживать пропуск навигации</span><span class="sxs-lookup"><span data-stu-id="e0e29-142">INavigationBase replaces INavigation in some APIs to support skip navigations</span></span>](#inavigationbase)                                     | <span data-ttu-id="e0e29-143">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-143">Low</span></span>        |
| <span data-ttu-id="e0e29-144">[Некоторые запросы с помощью коррелированной коллекции, которые также используют `Distinct` или `GroupBy`, больше не поддерживаются](#collection-distinct-groupby).</span><span class="sxs-lookup"><span data-stu-id="e0e29-144">[Some queries with correlated collection that also use `Distinct` or `GroupBy` are no longer supported](#collection-distinct-groupby)</span></span> | <span data-ttu-id="e0e29-145">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-145">Low</span></span>        |
| [<span data-ttu-id="e0e29-146">Использование коллекции запрашиваемого типа в проекции не поддерживается</span><span class="sxs-lookup"><span data-stu-id="e0e29-146">Using a collection of Queryable type in projection is not supported</span></span>](#queryable-projection)                                          | <span data-ttu-id="e0e29-147">Низкий</span><span class="sxs-lookup"><span data-stu-id="e0e29-147">Low</span></span>        |

## <a name="medium-impact-changes"></a><span data-ttu-id="e0e29-148">Изменения средней степени влияния</span><span class="sxs-lookup"><span data-stu-id="e0e29-148">Medium-impact changes</span></span>

<a name="required-dependent"></a>

### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="e0e29-149">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="e0e29-149">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="e0e29-150">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="e0e29-150">Tracking Issue #17286</span></span>](https://github.com/dotnet/efcore/issues/17286)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-151">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-151">Old behavior</span></span>

<span data-ttu-id="e0e29-152">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="e0e29-152">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="e0e29-153">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e0e29-153">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-154">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-154">New behavior</span></span>

<span data-ttu-id="e0e29-155">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="e0e29-155">With the added support for required dependents, it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="e0e29-156">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="e0e29-156">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

#### <a name="why"></a><span data-ttu-id="e0e29-157">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-157">Why</span></span>

<span data-ttu-id="e0e29-158">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="e0e29-158">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-159">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-159">Mitigations</span></span>

<span data-ttu-id="e0e29-160">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-160">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a><span data-ttu-id="e0e29-161">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="e0e29-161">Defining query is replaced with provider-specific methods</span></span>

[<span data-ttu-id="e0e29-162">Отслеживание вопроса № 18903</span><span class="sxs-lookup"><span data-stu-id="e0e29-162">Tracking Issue #18903</span></span>](https://github.com/dotnet/efcore/issues/18903)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-163">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-163">Old behavior</span></span>

<span data-ttu-id="e0e29-164">Типы сущностей сопоставлялись с определяющими запросами на уровне ядра.</span><span class="sxs-lookup"><span data-stu-id="e0e29-164">Entity types were mapped to defining queries at the Core level.</span></span> <span data-ttu-id="e0e29-165">Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.</span><span class="sxs-lookup"><span data-stu-id="e0e29-165">Anytime the entity type was used in the query root of the entity type was replaced by the defining query for any provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-166">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-166">New behavior</span></span>

<span data-ttu-id="e0e29-167">Интерфейсы API для определяющего запроса стали нерекомендуемыми.</span><span class="sxs-lookup"><span data-stu-id="e0e29-167">APIs for defining query are deprecated.</span></span> <span data-ttu-id="e0e29-168">Появились новые интерфейсы API для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="e0e29-168">New provider-specific APIs were introduced.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-169">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-169">Why</span></span>

<span data-ttu-id="e0e29-170">Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.</span><span class="sxs-lookup"><span data-stu-id="e0e29-170">While defining queries were implemented as replacement query whenever query root is used in the query, it had a few issues:</span></span>

- <span data-ttu-id="e0e29-171">Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.</span><span class="sxs-lookup"><span data-stu-id="e0e29-171">If defining query is projecting entity type using `new { ... }` in `Select` method, then identifying that as an entity required additional work and made it inconsistent with how EF Core treats nominal types in the query.</span></span>
- <span data-ttu-id="e0e29-172">Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.</span><span class="sxs-lookup"><span data-stu-id="e0e29-172">For relational providers `FromSql` is still needed to pass the SQL string in LINQ expression form.</span></span>

<span data-ttu-id="e0e29-173">Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных).</span><span class="sxs-lookup"><span data-stu-id="e0e29-173">Initially defining queries were introduced as client-side views to be used with In-Memory provider for keyless entities (similar to database views in relational databases).</span></span> <span data-ttu-id="e0e29-174">Такое определение упрощает тестирование приложения с базой данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="e0e29-174">Such definition makes it easy to test application against in-memory database.</span></span> <span data-ttu-id="e0e29-175">Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание.</span><span class="sxs-lookup"><span data-stu-id="e0e29-175">Afterwards they became broadly applicable, which was useful but brought inconsistent and hard to understand behavior.</span></span> <span data-ttu-id="e0e29-176">Поэтому мы решили упростить принцип их работы.</span><span class="sxs-lookup"><span data-stu-id="e0e29-176">So we decided to simplify the concept.</span></span> <span data-ttu-id="e0e29-177">Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому.</span><span class="sxs-lookup"><span data-stu-id="e0e29-177">We made LINQ based defining query exclusive to In-Memory provider and treat them differently.</span></span> <span data-ttu-id="e0e29-178">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).</span><span class="sxs-lookup"><span data-stu-id="e0e29-178">For more information, [see this issue](https://github.com/dotnet/efcore/issues/20023).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-179">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-179">Mitigations</span></span>

<span data-ttu-id="e0e29-180">Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e0e29-180">For relational providers, use `ToSqlQuery` method in `OnModelCreating` and pass in a SQL string to use for the entity type.</span></span>
<span data-ttu-id="e0e29-181">Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e0e29-181">For the In-Memory provider, use `ToInMemoryQuery` method in `OnModelCreating` and pass in a LINQ query to use for the entity type.</span></span>

<a name="nonnullreferences"></a>

### <a name="non-null-reference-navigations-are-not-overwritten-by-queries"></a><span data-ttu-id="e0e29-182">Навигации по ссылке, которая не является пустой, не переопределяются запросами</span><span class="sxs-lookup"><span data-stu-id="e0e29-182">Non-null reference navigations are not overwritten by queries</span></span>

[<span data-ttu-id="e0e29-183">Отслеживание проблемы № 2693</span><span class="sxs-lookup"><span data-stu-id="e0e29-183">Tracking Issue #2693</span></span>](https://github.com/dotnet/EntityFramework.Docs/issues/2693)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-184">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-184">Old behavior</span></span>

<span data-ttu-id="e0e29-185">В EF Core 3.1 навигации по ссылкам, которые изначально инициализированы значениями, отличными от NULL, иногда перезаписывались экземплярами сущностей из базы данных, независимо от того, совпадали ли значения ключей.</span><span class="sxs-lookup"><span data-stu-id="e0e29-185">In EF Core 3.1, reference navigations eagerly initialized to non-null values would sometimes be overwritten by entity instances from the database, regardless of whether or not key values matched.</span></span> <span data-ttu-id="e0e29-186">Однако в других случаях в EF Core 3.1 выполнялось обратное действие и оставлялось существующее значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="e0e29-186">However, in other cases, EF Core 3.1 would do the opposite and leave the existing non-null value.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-187">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-187">New behavior</span></span>

<span data-ttu-id="e0e29-188">Начиная с версии EF Core 5.0, навигации по пустой ссылке, которая не является пустой, никогда не перезаписываются экземплярами, возвращаемыми запросом.</span><span class="sxs-lookup"><span data-stu-id="e0e29-188">Starting with EF Core 5.0, non-null reference navigations are never overwritten by instances returned from a query.</span></span>

<span data-ttu-id="e0e29-189">Обратите внимание, что безотложная инициализация навигации по _коллекции_ для пустой коллекции по-прежнему поддерживается.</span><span class="sxs-lookup"><span data-stu-id="e0e29-189">Note that eager initialization of a _collection_ navigation to an empty collection is still supported.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-190">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-190">Why</span></span>

<span data-ttu-id="e0e29-191">Инициализация свойства навигации по ссылкам для "пустого" экземпляра сущности приводит к неоднозначному состоянию.</span><span class="sxs-lookup"><span data-stu-id="e0e29-191">Initialization of a reference navigation property to an "empty" entity instance results in an ambiguous state.</span></span> <span data-ttu-id="e0e29-192">Пример:</span><span class="sxs-lookup"><span data-stu-id="e0e29-192">For example:</span></span>

```csharp
public class Blog
{
     public int Id { get; set; }
     public Author Author { get; set; ) = new Author();
}
```

<span data-ttu-id="e0e29-193">Обычно запрос блогов и авторов сначала создает экземпляры `Blog`, а затем задает соответствующие экземпляры `Author` на основе данных, возвращаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-193">Normally a query for Blogs and Authors will first create `Blog` instances and then set the appropriate `Author` instances based on the data returned from the database.</span></span> <span data-ttu-id="e0e29-194">Однако в этом случае каждое свойство `Blog.Author` будет уже инициализировано для пустого свойства `Author`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-194">However, in this case every `Blog.Author` property is already initialized to an empty `Author`.</span></span> <span data-ttu-id="e0e29-195">Кроме EF Core, не существует способа выяснить, является ли этот экземпляр "пустым".</span><span class="sxs-lookup"><span data-stu-id="e0e29-195">Except EF Core has no way to know that this instance is "empty".</span></span> <span data-ttu-id="e0e29-196">Таким образом, перезапись этого экземпляра может без уведомления привести к выводу допустимого `Author`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-196">So overwriting this instance could potentially silently throw away a valid `Author`.</span></span> <span data-ttu-id="e0e29-197">Таким образом, EF Core 5.0 теперь не перезаписывает уже инициализированную навигацию.</span><span class="sxs-lookup"><span data-stu-id="e0e29-197">Therefore, EF Core 5.0 now consistently does not overwrite a navigation that is already initialized.</span></span>

<span data-ttu-id="e0e29-198">Это новое поведение также соответствует обычному поведению EF6, хотя при исследовании мы также обнаружили некоторые случаи несогласованности в EF6.</span><span class="sxs-lookup"><span data-stu-id="e0e29-198">This new behavior also aligns with the behavior of EF6 in most cases, although upon investigation we also found some cases of inconsistency in EF6.</span></span>  

#### <a name="mitigations"></a><span data-ttu-id="e0e29-199">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-199">Mitigations</span></span>

<span data-ttu-id="e0e29-200">В случае обнаружения такого прерывания исправление будет способствовать остановке активной инициализации свойств навигации по ссылкам.</span><span class="sxs-lookup"><span data-stu-id="e0e29-200">If this break is encountered, then the fix is to stop eagerly initializing reference navigation properties.</span></span>

## <a name="low-impact-changes"></a><span data-ttu-id="e0e29-201">Изменения низкой степени влияния</span><span class="sxs-lookup"><span data-stu-id="e0e29-201">Low-impact changes</span></span>

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="e0e29-202">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="e0e29-202">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="e0e29-203">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="e0e29-203">Tracking Issue #14257</span></span>](https://github.com/dotnet/efcore/issues/14257)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-204">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-204">Old behavior</span></span>

<span data-ttu-id="e0e29-205">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="e0e29-205">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="e0e29-206">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-206">However, it only ever affected database creation.</span></span> <span data-ttu-id="e0e29-207">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="e0e29-207">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="e0e29-208">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/dotnet/efcore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="e0e29-208">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/dotnet/efcore/issues/14257)).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-209">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-209">New behavior</span></span>

<span data-ttu-id="e0e29-210">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="e0e29-210">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="e0e29-211">Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="e0e29-211">This API matches more closely to the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-212">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-212">Why</span></span>

<span data-ttu-id="e0e29-213">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="e0e29-213">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-214">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-214">Mitigations</span></span>

<span data-ttu-id="e0e29-215">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="e0e29-215">Use `HasColumnType` to specify the dimension:</span></span>

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

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a><span data-ttu-id="e0e29-216">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="e0e29-216">Cosmos: Partition key is now added to the primary key</span></span>

[<span data-ttu-id="e0e29-217">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="e0e29-217">Tracking Issue #15289</span></span>](https://github.com/dotnet/efcore/issues/15289)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-218">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-218">Old behavior</span></span>

<span data-ttu-id="e0e29-219">Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-219">The partition key property was only added to the alternate key that includes `id`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-220">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-220">New behavior</span></span>

<span data-ttu-id="e0e29-221">В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="e0e29-221">The partition key property is now also added to the primary key by convention.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-222">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-222">Why</span></span>

<span data-ttu-id="e0e29-223">Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="e0e29-223">This change makes the model better aligned with Azure Cosmos DB semantics and improves the performance of `Find` and some queries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-224">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-224">Mitigations</span></span>

<span data-ttu-id="e0e29-225">Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-225">To prevent the partition key property to be added to the primary key, configure it in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a><span data-ttu-id="e0e29-226">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="e0e29-226">Cosmos: `id` property renamed to `__id`</span></span>

[<span data-ttu-id="e0e29-227">Отслеживание вопроса № 17751</span><span class="sxs-lookup"><span data-stu-id="e0e29-227">Tracking Issue #17751</span></span>](https://github.com/dotnet/efcore/issues/17751)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-228">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-228">Old behavior</span></span>

<span data-ttu-id="e0e29-229">Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-229">The shadow property mapped to the `id` JSON property was also named `id`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-230">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-230">New behavior</span></span>

<span data-ttu-id="e0e29-231">Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-231">The shadow property created by convention is now named `__id`.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-232">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-232">Why</span></span>

<span data-ttu-id="e0e29-233">Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e0e29-233">This change makes it less likely that the `id` property clashes with an existing property on the entity type.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-234">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-234">Mitigations</span></span>

<span data-ttu-id="e0e29-235">Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-235">To go back to the 3.x behavior, configure the `id` property in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a><span data-ttu-id="e0e29-236">Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива</span><span class="sxs-lookup"><span data-stu-id="e0e29-236">Cosmos: byte[] is now stored as a base64 string instead of a number array</span></span>

[<span data-ttu-id="e0e29-237">Отслеживание вопроса № 17306</span><span class="sxs-lookup"><span data-stu-id="e0e29-237">Tracking Issue #17306</span></span>](https://github.com/dotnet/efcore/issues/17306)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-238">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-238">Old behavior</span></span>

<span data-ttu-id="e0e29-239">Свойства типа byte[] хранились в виде числового массива.</span><span class="sxs-lookup"><span data-stu-id="e0e29-239">Properties of type byte[] were stored as a number array.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-240">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-240">New behavior</span></span>

<span data-ttu-id="e0e29-241">Свойства типа byte[] теперь хранятся в виде строки base64.</span><span class="sxs-lookup"><span data-stu-id="e0e29-241">Properties of type byte[] are now stored as a base64 string.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-242">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-242">Why</span></span>

<span data-ttu-id="e0e29-243">Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.</span><span class="sxs-lookup"><span data-stu-id="e0e29-243">This representation of byte[] aligns better with expectations and is the default behavior of the major JSON serialization libraries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-244">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-244">Mitigations</span></span>

<span data-ttu-id="e0e29-245">Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки.</span><span class="sxs-lookup"><span data-stu-id="e0e29-245">Existing data stored as number arrays will still be queried correctly, but currently there isn't a supported way to change back the insert behavior.</span></span> <span data-ttu-id="e0e29-246">Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/dotnet/efcore/issues/17306)</span><span class="sxs-lookup"><span data-stu-id="e0e29-246">If this limitation is blocking your scenario, comment on [this issue](https://github.com/dotnet/efcore/issues/17306)</span></span>

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a><span data-ttu-id="e0e29-247">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="e0e29-247">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>

[<span data-ttu-id="e0e29-248">Отслеживание вопроса № 17874</span><span class="sxs-lookup"><span data-stu-id="e0e29-248">Tracking Issue #17874</span></span>](https://github.com/dotnet/efcore/issues/17874)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-249">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-249">Old behavior</span></span>

<span data-ttu-id="e0e29-250">Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-250">Previously the extension methods were called `GetPropertyName` and `SetPropertyName`</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-251">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-251">New behavior</span></span>

<span data-ttu-id="e0e29-252">Старый интерфейс API был удален, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-252">The old API was removed and new methods added: `GetJsonPropertyName`, `SetJsonPropertyName`</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-253">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-253">Why</span></span>

<span data-ttu-id="e0e29-254">Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.</span><span class="sxs-lookup"><span data-stu-id="e0e29-254">This change removes the ambiguity around what these methods are configuring.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-255">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-255">Mitigations</span></span>

<span data-ttu-id="e0e29-256">Используйте новый API.</span><span class="sxs-lookup"><span data-stu-id="e0e29-256">Use the new API.</span></span>

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="e0e29-257">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="e0e29-257">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>

[<span data-ttu-id="e0e29-258">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="e0e29-258">Tracking Issue #15289</span></span>](https://github.com/dotnet/efcore/issues/15289)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-259">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-259">Old behavior</span></span>

<span data-ttu-id="e0e29-260">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="e0e29-260">Value generators were only called when the entity state changed to Added.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-261">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-261">New behavior</span></span>

<span data-ttu-id="e0e29-262">Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e0e29-262">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated, or Deleted and the property contains the default values.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-263">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-263">Why</span></span>

<span data-ttu-id="e0e29-264">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="e0e29-264">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-265">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-265">Mitigations</span></span>

<span data-ttu-id="e0e29-266">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="e0e29-266">To prevent the value generator from being called, assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>

### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="e0e29-267">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="e0e29-267">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="e0e29-268">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="e0e29-268">Tracking Issue #20305</span></span>](https://github.com/dotnet/efcore/issues/20305)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-269">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-269">Old behavior</span></span>

<span data-ttu-id="e0e29-270">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-270">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-271">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-271">New behavior</span></span>

<span data-ttu-id="e0e29-272">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-272">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="e0e29-273">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="e0e29-273">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-274">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-274">Why</span></span>

<span data-ttu-id="e0e29-275">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-275">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="e0e29-276">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="e0e29-276">Using it to find differences is faster and more accurate.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-277">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-277">Mitigations</span></span>

<span data-ttu-id="e0e29-278">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-278">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

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

<span data-ttu-id="e0e29-279">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="e0e29-279">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="toview"></a>

### <a name="toview-is-treated-differently-by-migrations"></a><span data-ttu-id="e0e29-280">ToView() по-разному обрабатывается миграциями</span><span class="sxs-lookup"><span data-stu-id="e0e29-280">ToView() is treated differently by migrations</span></span>

[<span data-ttu-id="e0e29-281">Отслеживание проблемы № 2725</span><span class="sxs-lookup"><span data-stu-id="e0e29-281">Tracking Issue #2725</span></span>](https://github.com/dotnet/efcore/issues/2725)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-282">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-282">Old behavior</span></span>

<span data-ttu-id="e0e29-283">Вызов `ToView(string)` заставляет миграции игнорировать тип сущности в дополнение к сопоставлению его с представлением.</span><span class="sxs-lookup"><span data-stu-id="e0e29-283">Calling `ToView(string)` made the migrations ignore the entity type in addition to mapping it to a view.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-284">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-284">New behavior</span></span>

<span data-ttu-id="e0e29-285">Теперь `ToView(string)` помечает тип сущности как не сопоставленный с таблицей в дополнение к сопоставлению его с представлением.</span><span class="sxs-lookup"><span data-stu-id="e0e29-285">Now `ToView(string)` marks the entity type as not mapped to a table in addition to mapping it to a view.</span></span> <span data-ttu-id="e0e29-286">Это приводит к тому, что при первой миграции после обновления до EF Core 5 предпринимается попытка удаления таблицы по умолчанию для этого типа сущности, так как он больше не игнорируется.</span><span class="sxs-lookup"><span data-stu-id="e0e29-286">This results in the first migration after upgrading to EF Core 5 to try to drop the default table for this entity type as it's not longer ignored.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-287">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-287">Why</span></span>

<span data-ttu-id="e0e29-288">EF Core теперь позволяет сопоставлять тип сущности и с таблицей, и с представлением одновременно, поэтому `ToView` больше нельзя использовать для указания, что тип сущности следует игнорировать при миграции.</span><span class="sxs-lookup"><span data-stu-id="e0e29-288">EF Core now allows an entity type to be mapped to both a table and a view simultaneously, so `ToView` is no longer a valid indicator that it should be ignored by migrations.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-289">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-289">Mitigations</span></span>

<span data-ttu-id="e0e29-290">Используйте следующий код, чтобы пометить сопоставленную таблицу как исключенную из миграции:</span><span class="sxs-lookup"><span data-stu-id="e0e29-290">Use the following code to mark the mapped table as excluded from migrations:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<User>().ToTable("UserView", t => t.ExcludeFromMigrations());
}
```

<a name="totable"></a>

### <a name="totablenull-marks-the-entity-type-as-not-mapped-to-a-table"></a><span data-ttu-id="e0e29-291">ToTable(null) помечает тип сущности как не сопоставленный с таблицей</span><span class="sxs-lookup"><span data-stu-id="e0e29-291">ToTable(null) marks the entity type as not mapped to a table</span></span>

[<span data-ttu-id="e0e29-292">Отслеживание проблемы № 21172</span><span class="sxs-lookup"><span data-stu-id="e0e29-292">Tracking Issue #21172</span></span>](https://github.com/dotnet/efcore/issues/21172)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-293">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-293">Old behavior</span></span>

<span data-ttu-id="e0e29-294">`ToTable(null)` сбрасывает имя таблицы в значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e0e29-294">`ToTable(null)` would reset the table name to the default.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-295">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-295">New behavior</span></span>

<span data-ttu-id="e0e29-296">`ToTable(null)` теперь помечает тип сущности как не сопоставленный с какой-либо таблицей.</span><span class="sxs-lookup"><span data-stu-id="e0e29-296">`ToTable(null)` now marks the entity type as not mapped to any table.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-297">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-297">Why</span></span>

<span data-ttu-id="e0e29-298">EF Core теперь позволяет сопоставлять тип сущности и с таблицей, и с представлением одновременно, поэтому `ToTable(null)` используется для указания, что тип сущности не сопоставлен ни с какой таблицей.</span><span class="sxs-lookup"><span data-stu-id="e0e29-298">EF Core now allows an entity type to be mapped to both a table and a view simultaneously, so `ToTable(null)` is used to indicate that it isn't mapped to any table.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-299">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-299">Mitigations</span></span>

<span data-ttu-id="e0e29-300">Используйте следующий код, чтобы сбрасывать имя таблицы в значение по умолчанию, если отсутствует сопоставление с представлением или DbFunction:</span><span class="sxs-lookup"><span data-stu-id="e0e29-300">Use the following code to reset the table name to the default if it's not mapped to a view or a DbFunction:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<User>().Metadata.RemoveAnnotation(RelationalAnnotationNames.TableName);
}
```

<a name="read-only-discriminators"></a>

### <a name="discriminators-are-read-only"></a><span data-ttu-id="e0e29-301">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="e0e29-301">Discriminators are read-only</span></span>

[<span data-ttu-id="e0e29-302">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="e0e29-302">Tracking Issue #21154</span></span>](https://github.com/dotnet/efcore/issues/21154)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-303">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-303">Old behavior</span></span>

<span data-ttu-id="e0e29-304">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="e0e29-304">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-305">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-305">New behavior</span></span>

<span data-ttu-id="e0e29-306">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="e0e29-306">An exception will be throws in the above case.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-307">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-307">Why</span></span>

<span data-ttu-id="e0e29-308">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="e0e29-308">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state, which might result in unexpected behavior.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-309">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-309">Mitigations</span></span>

<span data-ttu-id="e0e29-310">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="e0e29-310">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges`, the discriminator can be made mutable:</span></span>

```csharp
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```

<a name="no-client-methods"></a>

### <a name="provider-specific-effunctions-methods-throw-for-inmemory-provider"></a><span data-ttu-id="e0e29-311">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="e0e29-311">Provider-specific EF.Functions methods throw for InMemory provider</span></span>

[<span data-ttu-id="e0e29-312">Отслеживание вопроса № 20294</span><span class="sxs-lookup"><span data-stu-id="e0e29-312">Tracking Issue #20294</span></span>](https://github.com/dotnet/efcore/issues/20294)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-313">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-313">Old behavior</span></span>

<span data-ttu-id="e0e29-314">Относящиеся к конкретному поставщику методы EF.Functions содержали реализацию для выполнения на стороне клиента, что позволяло им выполняться в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="e0e29-314">Provider-specific EF.Functions methods contained implementation for client execution, which allowed them to be executed on the InMemory provider.</span></span> <span data-ttu-id="e0e29-315">Например, `EF.Functions.DateDiffDay` — это относящийся к SQL Server метод, который работал в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="e0e29-315">For example, `EF.Functions.DateDiffDay` is a Sql Server specific method, which worked on InMemory provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-316">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-316">New behavior</span></span>

<span data-ttu-id="e0e29-317">Методы, относящиеся к конкретному поставщику, были изменены так, чтобы в теле метода создавалось исключение, блокирующее вычисление метода на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="e0e29-317">Provider-specific methods have been updated to throw an exception in their method body to block evaluating them on client side.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-318">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-318">Why</span></span>

<span data-ttu-id="e0e29-319">Методы, относящиеся к конкретному поставщику, сопоставляются с функцией базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-319">Provider-specific methods map to a database function.</span></span> <span data-ttu-id="e0e29-320">Вычисление, выполняемое сопоставленной функцией базы данных, не всегда может реплицироваться на стороне клиента в LINQ.</span><span class="sxs-lookup"><span data-stu-id="e0e29-320">The computation done by the mapped database function can't always be replicated on the client side in LINQ.</span></span> <span data-ttu-id="e0e29-321">Поэтому результаты, полученные при выполнении одного и того же метода на сервере и в клиенте, могут различаться.</span><span class="sxs-lookup"><span data-stu-id="e0e29-321">It may cause the result from the server to differ when executing the same method on client.</span></span> <span data-ttu-id="e0e29-322">Так как эти методы используются в LINQ для преобразования в определенные функции базы данных, их не нужно вычислять на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="e0e29-322">Since these methods are used in LINQ to translate to specific database functions, they don't need to be evaluated on client side.</span></span> <span data-ttu-id="e0e29-323">Так как поставщик InMemory — это другая *база данных*, эти методы недоступны для него.</span><span class="sxs-lookup"><span data-stu-id="e0e29-323">As InMemory provider is a different *database*, these methods aren't available for this provider.</span></span> <span data-ttu-id="e0e29-324">При попытке выполнить их для поставщика InMemory или любого другого поставщика, который не преобразует эти методы, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="e0e29-324">Trying to execute them for InMemory provider, or any other provider that doesn't translate these methods, throws an exception.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-325">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-325">Mitigations</span></span>

<span data-ttu-id="e0e29-326">Так как точно воссоздать поведение функций базы данных невозможно, содержащие их запросы следует тестировать на основе базы данных того же типа, что и рабочая база данных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-326">Since there's no way to mimic behavior of database functions accurately, you should test the queries containing them against same kind of database as in production.</span></span>

<a name="getcolumnname-obsolete"></a>

### <a name="ipropertygetcolumnname-is-now-obsolete"></a><span data-ttu-id="e0e29-327">IProperty.GetColumnName() считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="e0e29-327">IProperty.GetColumnName() is now obsolete</span></span>

[<span data-ttu-id="e0e29-328">Отслеживание проблемы № 2266</span><span class="sxs-lookup"><span data-stu-id="e0e29-328">Tracking Issue #2266</span></span>](https://github.com/dotnet/efcore/issues/2266)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-329">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-329">Old behavior</span></span>

<span data-ttu-id="e0e29-330">`GetColumnName()` возвращал имя столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="e0e29-330">`GetColumnName()` returned the name of the column that a property is mapped to.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-331">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-331">New behavior</span></span>

<span data-ttu-id="e0e29-332">`GetColumnName()` по-прежнему возвращает имя столбца, с которым сопоставлено свойство, но это поведение стало неоднозначным, так как EF Core 5 поддерживает TPT и одновременное сопоставление с представлением или функцией, где эти сопоставления могут использовать разные имена столбцов для одного и того же свойства.</span><span class="sxs-lookup"><span data-stu-id="e0e29-332">`GetColumnName()` still returns the name of a column that a property is mapped to, but this behavior is now ambiguous since EF Core 5 supports TPT and simultaneous mapping to a view or a function where these mappings could use different column names for the same property.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-333">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-333">Why</span></span>

<span data-ttu-id="e0e29-334">Мы пометили этот метод как устаревший, чтобы помочь пользователям более точно перегружать <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName(Microsoft.EntityFrameworkCore.Metadata.IProperty,Microsoft.EntityFrameworkCore.Metadata.StoreObjectIdentifier@)>.</span><span class="sxs-lookup"><span data-stu-id="e0e29-334">We marked this method as obsolete to guide users to a more accurate overload - <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName(Microsoft.EntityFrameworkCore.Metadata.IProperty,Microsoft.EntityFrameworkCore.Metadata.StoreObjectIdentifier@)>.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-335">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-335">Mitigations</span></span>

<span data-ttu-id="e0e29-336">Используйте следующий код, чтобы получить имя столбца для определенной таблицы:</span><span class="sxs-lookup"><span data-stu-id="e0e29-336">Use the following code to get the column name for a specific table:</span></span>

```csharp
var columnName = property.GetColumnName(StoreObjectIdentifier.Table("Users", null)));
```

<a name="index-obsolete"></a>

### <a name="indexbuilderhasname-is-now-obsolete"></a><span data-ttu-id="e0e29-337">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="e0e29-337">IndexBuilder.HasName is now obsolete</span></span>

[<span data-ttu-id="e0e29-338">Отслеживание проблемы № 21089</span><span class="sxs-lookup"><span data-stu-id="e0e29-338">Tracking Issue #21089</span></span>](https://github.com/dotnet/efcore/issues/21089)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-339">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-339">Old behavior</span></span>

<span data-ttu-id="e0e29-340">Ранее допускалось определение только одного индекса для конкретного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="e0e29-340">Previously, only one index could be defined over a given set of properties.</span></span> <span data-ttu-id="e0e29-341">Имя для базы данных индекса настраивалось с использованием IndexBuilder.HasName.</span><span class="sxs-lookup"><span data-stu-id="e0e29-341">The database name of an index was configured using IndexBuilder.HasName.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-342">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-342">New behavior</span></span>

<span data-ttu-id="e0e29-343">Теперь допускается несколько индексов для одного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="e0e29-343">Multiple indexes are now allowed on the same set or properties.</span></span> <span data-ttu-id="e0e29-344">Эти индексы отличаются именами в пределах модели.</span><span class="sxs-lookup"><span data-stu-id="e0e29-344">These indexes are now distinguished by a name in the model.</span></span> <span data-ttu-id="e0e29-345">По соглашению имя модели используется в качестве имени базы данных, но его также можно указать независимо с использованием HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="e0e29-345">By convention, the model name is used as the database name; however it can also be configured independently using HasDatabaseName.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-346">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-346">Why</span></span>

<span data-ttu-id="e0e29-347">В будущем мы хотим включить для набора свойств параллельные индексы с сортировкой по возрастанию и убыванию, а также индексы с разными параметрами сортировки.</span><span class="sxs-lookup"><span data-stu-id="e0e29-347">In the future, we'd like to enable both ascending and descending indexes or indexes with different collations on the same set of properties.</span></span> <span data-ttu-id="e0e29-348">Это изменение продвигает нас в этом направлении.</span><span class="sxs-lookup"><span data-stu-id="e0e29-348">This change moves us another step in that direction.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-349">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-349">Mitigations</span></span>

<span data-ttu-id="e0e29-350">Любой код, который ранее вызывал IndexBuilder.HasName, необходимо изменить для вызова HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="e0e29-350">Any code that was previously calling IndexBuilder.HasName should be updated to call HasDatabaseName instead.</span></span>

<span data-ttu-id="e0e29-351">Если в проекте есть миграции, созданные до версии EF Core 2.0.0, вы можете игнорировать предупреждения для этих файлов, подавив их с помощью `#pragma warning disable 612, 618`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-351">If your project includes migrations generated prior to EF Core version 2.0.0, you can safely ignore the warning in those files and suppress it by adding `#pragma warning disable 612, 618`.</span></span>

<a name="pluralizer"></a>

### <a name="a-pluralizer-is-now-included-for-scaffolding-reverse-engineered-models"></a><span data-ttu-id="e0e29-352">Добавлено средство преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="e0e29-352">A pluralizer is now included for scaffolding reverse engineered models</span></span>

[<span data-ttu-id="e0e29-353">Отслеживание проблемы № 11160</span><span class="sxs-lookup"><span data-stu-id="e0e29-353">Tracking Issue #11160</span></span>](https://github.com/dotnet/efcore/issues/11160)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-354">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-354">Old behavior</span></span>

<span data-ttu-id="e0e29-355">Ранее нужно было устанавливать отдельный пакет, чтобы преобразовывать имена DbSet и коллекций во множественное число и имена таблиц в единственное число при создании шаблонов DbContext и типов сущностей путем реконструирования из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-355">Previously, you had to install a separate pluralizer package in order to pluralize DbSet and collection navigation names and singularize table names when scaffolding a DbContext and entity types by reverse engineering a database schema.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-356">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-356">New behavior</span></span>

<span data-ttu-id="e0e29-357">Теперь EF Core содержит собственное средство для преобразования во множественное число на основе библиотеки [Humanizer](https://github.com/Humanizr/Humanizer).</span><span class="sxs-lookup"><span data-stu-id="e0e29-357">EF Core now includes a pluralizer that uses the [Humanizer](https://github.com/Humanizr/Humanizer) library.</span></span> <span data-ttu-id="e0e29-358">Именно эту библиотеку использует Visual Studio, рекомендуя имена переменных.</span><span class="sxs-lookup"><span data-stu-id="e0e29-358">This is the same library Visual Studio uses to recommend variable names.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-359">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-359">Why</span></span>

<span data-ttu-id="e0e29-360">Использование форм множественного числа для свойств коллекций и единственного числа для типов и ссылочных свойств считается идиоматичным в .NET.</span><span class="sxs-lookup"><span data-stu-id="e0e29-360">Using plural forms of words for collection properties and singular forms for types and reference properties is idiomatic in .NET.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-361">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-361">Mitigations</span></span>

<span data-ttu-id="e0e29-362">Чтобы отключить средство для преобразования во множественное число, укажите параметр `--no-pluralize` для `dotnet ef dbcontext scaffold` или `-NoPluralize` для `Scaffold-DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-362">To disable the pluralizer, use the `--no-pluralize` option on `dotnet ef dbcontext scaffold` or the `-NoPluralize` switch on `Scaffold-DbContext`.</span></span>

<a name="inavigationbase"></a>

### <a name="inavigationbase-replaces-inavigation-in-some-apis-to-support-skip-navigations"></a><span data-ttu-id="e0e29-363">INavigationBase заменяет INavigation в некоторых API, чтобы поддерживать пропуск навигации</span><span class="sxs-lookup"><span data-stu-id="e0e29-363">INavigationBase replaces INavigation in some APIs to support skip navigations</span></span>

[<span data-ttu-id="e0e29-364">Отслеживание проблемы № 2568</span><span class="sxs-lookup"><span data-stu-id="e0e29-364">Tracking Issue #2568</span></span>](https://github.com/dotnet/EntityFramework.Docs/issues/2568)

#### <a name="old-behavior"></a><span data-ttu-id="e0e29-365">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-365">Old behavior</span></span>

<span data-ttu-id="e0e29-366">В версиях EF Core, предшествующих версии 5.0, поддерживалась только одна форма свойства навигации, представленная интерфейсом `INavigation`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-366">EF Core prior to 5.0 supported only one form of navigation property, represented by the `INavigation` interface.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="e0e29-367">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="e0e29-367">New behavior</span></span>

<span data-ttu-id="e0e29-368">EF Core 5.0 представляет связи "многие ко многим", которые используют "пропуск навигации".</span><span class="sxs-lookup"><span data-stu-id="e0e29-368">EF Core 5.0 introduces many-to-many relationships which use "skip navigations".</span></span> <span data-ttu-id="e0e29-369">Они представлены интерфейсом `ISkipNavigation`, а большинство функциональных возможностей `INavigation` переданы в общий базовый интерфейс: `INavigationBase`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-369">These are represented by the `ISkipNavigation` interface, and most of the functionality of `INavigation` has been pushed down to a common base interface: `INavigationBase`.</span></span>

#### <a name="why"></a><span data-ttu-id="e0e29-370">Почему</span><span class="sxs-lookup"><span data-stu-id="e0e29-370">Why</span></span>

<span data-ttu-id="e0e29-371">Большая часть функций между обычной навигацией и пропуском навигации одинаковы.</span><span class="sxs-lookup"><span data-stu-id="e0e29-371">Most of the functionality between normal and skip navigations is the same.</span></span> <span data-ttu-id="e0e29-372">Тем не менее пропуск навигации имеет другую связь с внешними ключами по сравнению с обычной навигацией, поскольку вовлеченные внешние ключи находятся не на обоих концах связи, а скорее в объекте соединения.</span><span class="sxs-lookup"><span data-stu-id="e0e29-372">However, skip navigations have a different relationship to foreign keys than normal navigations, since the FKs involved are not directly on either end of the relationship, but rather in the join entity.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="e0e29-373">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="e0e29-373">Mitigations</span></span>

<span data-ttu-id="e0e29-374">Во многих случаях приложения могут переключиться на использование нового базового интерфейса, не нуждаясь в дополнительных изменениях.</span><span class="sxs-lookup"><span data-stu-id="e0e29-374">In many cases applications can switch to using the new base interface with no other changes.</span></span> <span data-ttu-id="e0e29-375">Однако если навигация используется для доступа к свойствам внешнего ключа, код приложения нужно либо ограничить только обычными переходами, либо обновить, чтобы он мог выполнять соответствующие действия как для обычной, так и для пропускной навигации.</span><span class="sxs-lookup"><span data-stu-id="e0e29-375">However, in cases where the navigation is used to access foreign key properties, application code should either be constrained to only normal navigations, or updated to do the appropriate thing for both normal and skip navigations.</span></span>

<a name="collection-distinct-groupby"></a>

### <a name="some-queries-with-correlated-collection-that-also-use-distinct-or-groupby-are-no-longer-supported"></a><span data-ttu-id="e0e29-376">Некоторые запросы с помощью коррелированной коллекции, которые также используют `Distinct` или `GroupBy`, больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="e0e29-376">Some queries with correlated collection that also use `Distinct` or `GroupBy` are no longer supported</span></span>

[<span data-ttu-id="e0e29-377">Отслеживание проблемы № 15873</span><span class="sxs-lookup"><span data-stu-id="e0e29-377">Tracking Issue #15873</span></span>](https://github.com/dotnet/efcore/issues/15873)

<span data-ttu-id="e0e29-378">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e0e29-378">**Old behavior**</span></span>

<span data-ttu-id="e0e29-379">Ранее мы позволяли выполнять запросы, включающие коррелированные коллекции с последующим свойством `GroupBy` и некоторые запросы с использованием `Distinct`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-379">Previously, queries involving correlated collections followed by `GroupBy`, as well as some queries using `Distinct` we allowed to execute.</span></span>

<span data-ttu-id="e0e29-380">Пример GroupBy:</span><span class="sxs-lookup"><span data-stu-id="e0e29-380">GroupBy example:</span></span>

```csharp
context.Parents
    .Select(p => p.Children
        .GroupBy(c => c.School)
        .Select(g => g.Key))
```

<span data-ttu-id="e0e29-381">Пример `Distinct`, в частности запросы `Distinct`, в которых внутренняя проекция коллекции не содержит первичный ключ:</span><span class="sxs-lookup"><span data-stu-id="e0e29-381">`Distinct` example - specifically `Distinct` queries where inner collection projection doesn't contain the primary key:</span></span>

```csharp
context.Parents
    .Select(p => p.Children
        .Select(c => c.School)
        .Distinct())
```

<span data-ttu-id="e0e29-382">Эти запросы могли возвращать неверные результаты, если внутренняя коллекция содержала дубликаты, однако работала должным образом, если все элементы во внутренней коллекции являлись уникальными.</span><span class="sxs-lookup"><span data-stu-id="e0e29-382">These queries could return incorrect results if the inner collection contained any duplicates, but worked correctly if all the elements in the inner collection were unique.</span></span>

<span data-ttu-id="e0e29-383">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e0e29-383">**New behavior**</span></span>

<span data-ttu-id="e0e29-384">Эти запросы больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="e0e29-384">These queries are no longer supported.</span></span> <span data-ttu-id="e0e29-385">Создается исключение, указывающее, что у нас недостаточно сведений для правильного создания результатов.</span><span class="sxs-lookup"><span data-stu-id="e0e29-385">Exception is thrown indicating that we don't have enough information to correctly build the results.</span></span>

<span data-ttu-id="e0e29-386">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e0e29-386">**Why**</span></span>

<span data-ttu-id="e0e29-387">В сценариях с коррелированными коллекциями, чтобы назначить сущность коллекции правильному родительскому объекту, нужно знать первичный ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="e0e29-387">For correlated collection scenarios we need to know entity's primary key in order to assign collection entities to the correct parent.</span></span> <span data-ttu-id="e0e29-388">Если внутренняя коллекция не использует `GroupBy` или `Distinct`, отсутствующий первичный ключ можно просто добавить в проекцию.</span><span class="sxs-lookup"><span data-stu-id="e0e29-388">When inner collection doesn't use `GroupBy` or `Distinct`, the missing primary key can simply be added to the projection.</span></span> <span data-ttu-id="e0e29-389">Однако при использовании `GroupBy` и `Distinct` это не удастся сделать, так как такое действие приведет к изменению результата операции `GroupBy` или `Distinct`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-389">However in case of `GroupBy` and `Distinct` it can't be done because it would change the result of `GroupBy` or `Distinct` operation.</span></span>

<span data-ttu-id="e0e29-390">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e0e29-390">**Mitigations**</span></span>

<span data-ttu-id="e0e29-391">Перепишите запрос, чтобы не использовать операции `GroupBy` или `Distinct` во внутренней коллекции, и выполните эти операции на клиенте.</span><span class="sxs-lookup"><span data-stu-id="e0e29-391">Rewrite the query to not use `GroupBy` or `Distinct` operations on the inner collection, and perform these operations on the client instead.</span></span>

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

### <a name="using-a-collection-of-queryable-type-in-projection-is-not-supported"></a><span data-ttu-id="e0e29-392">Использование коллекции запрашиваемого типа в проекции не поддерживается</span><span class="sxs-lookup"><span data-stu-id="e0e29-392">Using a collection of Queryable type in projection is not supported</span></span>

[<span data-ttu-id="e0e29-393">Отслеживание проблемы № 16314</span><span class="sxs-lookup"><span data-stu-id="e0e29-393">Tracking Issue #16314</span></span>](https://github.com/dotnet/efcore/issues/16314)

<span data-ttu-id="e0e29-394">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="e0e29-394">**Old behavior**</span></span>

<span data-ttu-id="e0e29-395">Ранее в отдельных случаях коллекцию запрашиваемых типов можно было использовать в проекции, например как аргумент для конструктора `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="e0e29-395">Previously, it was possible to use collection of a Queryable type inside the projection in some cases, for example as an argument to a `List<T>` constructor:</span></span>

```csharp
context.Blogs
    .Select(b => new List<Post>(context.Posts.Where(p => p.BlogId == b.Id)))
```

<span data-ttu-id="e0e29-396">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="e0e29-396">**New behavior**</span></span>

<span data-ttu-id="e0e29-397">Эти запросы больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="e0e29-397">These queries are no longer supported.</span></span> <span data-ttu-id="e0e29-398">Создается исключение с указанием на невозможность создания объекта запрашиваемого типа и предложением того, как это можно исправить.</span><span class="sxs-lookup"><span data-stu-id="e0e29-398">Exception is thrown indicating that we can't create an object of Queryable type and suggesting how this could be fixed.</span></span>

<span data-ttu-id="e0e29-399">**Причина**</span><span class="sxs-lookup"><span data-stu-id="e0e29-399">**Why**</span></span>

<span data-ttu-id="e0e29-400">Мы не можем реализовать объект запрашиваемого типа, поэтому такие объекты автоматически создавались с помощью типа `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="e0e29-400">We can't materialize an object of a Queryable type, so they would automatically be created using `List<T>` type instead.</span></span> <span data-ttu-id="e0e29-401">Это часто приводило к выводу исключения из-за несоответствия типов, которое было непонятным и могло быть неожиданным для некоторых пользователей.</span><span class="sxs-lookup"><span data-stu-id="e0e29-401">This would often cause an exception due to type mismatch which was not very clear and could be surprising to some users.</span></span> <span data-ttu-id="e0e29-402">Мы решили распознать шаблон и выдать более осмысленное исключение.</span><span class="sxs-lookup"><span data-stu-id="e0e29-402">We decided to recognize the pattern and throw a more meaningful exception.</span></span>

<span data-ttu-id="e0e29-403">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="e0e29-403">**Mitigations**</span></span>

<span data-ttu-id="e0e29-404">Добавьте вызов `ToList()` после запрашиваемого объекта в проекции:</span><span class="sxs-lookup"><span data-stu-id="e0e29-404">Add `ToList()` call after the Queryable object in the projection:</span></span>

```csharp
context.Blogs.Select(b => context.Posts.Where(p => p.BlogId == b.Id).ToList())
```
