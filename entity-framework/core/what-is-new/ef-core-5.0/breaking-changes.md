---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 11/07/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: e2537dbc1d5dba48450bd0fea7712054ba2fa622
ms.sourcegitcommit: 42bbf7f68e92c364c5fff63092d3eb02229f568d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94503180"
---
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="833d2-103">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="833d2-103">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="833d2-104">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="833d2-104">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="833d2-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="833d2-105">Summary</span></span>

| <span data-ttu-id="833d2-106">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="833d2-106">**Breaking change**</span></span>                                                                                                                   | <span data-ttu-id="833d2-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="833d2-107">**Impact**</span></span> |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="833d2-108">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="833d2-108">Required on the navigation from principal to dependent has different semantics</span></span>](#required-dependent)                                 | <span data-ttu-id="833d2-109">Средний</span><span class="sxs-lookup"><span data-stu-id="833d2-109">Medium</span></span>     |
| [<span data-ttu-id="833d2-110">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="833d2-110">Defining query is replaced with provider-specific methods</span></span>](#defining-query)                                                          | <span data-ttu-id="833d2-111">Средний</span><span class="sxs-lookup"><span data-stu-id="833d2-111">Medium</span></span>     |
| [<span data-ttu-id="833d2-112">Навигации по ссылке, которая не является пустой, не переопределяются запросами</span><span class="sxs-lookup"><span data-stu-id="833d2-112">Non-null reference navigations are not overwritten by queries</span></span>](#nonnullreferences)                                                   | <span data-ttu-id="833d2-113">Средний</span><span class="sxs-lookup"><span data-stu-id="833d2-113">Medium</span></span>     |
| [<span data-ttu-id="833d2-114">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="833d2-114">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#geometric-sqlite)                                                   | <span data-ttu-id="833d2-115">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-115">Low</span></span>        |
| [<span data-ttu-id="833d2-116">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="833d2-116">Cosmos: Partition key is now added to the primary key</span></span>](#cosmos-partition-key)                                                        | <span data-ttu-id="833d2-117">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-117">Low</span></span>        |
| [<span data-ttu-id="833d2-118">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="833d2-118">Cosmos: `id` property renamed to `__id`</span></span>](#cosmos-id)                                                                                 | <span data-ttu-id="833d2-119">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-119">Low</span></span>        |
| <span data-ttu-id="833d2-120">[Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)</span><span class="sxs-lookup"><span data-stu-id="833d2-120">[Cosmos: byte[] is now stored as a base64 string instead of a number array](#cosmos-byte)</span></span>                                             | <span data-ttu-id="833d2-121">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-121">Low</span></span>        |
| [<span data-ttu-id="833d2-122">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="833d2-122">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>](#cosmos-metadata)                                                          | <span data-ttu-id="833d2-123">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-123">Low</span></span>        |
| [<span data-ttu-id="833d2-124">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="833d2-124">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>](#non-added-generation) | <span data-ttu-id="833d2-125">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-125">Low</span></span>        |
| [<span data-ttu-id="833d2-126">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="833d2-126">IMigrationsModelDiffer now uses IRelationalModel</span></span>](#relational-model)                                                                 | <span data-ttu-id="833d2-127">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-127">Low</span></span>        |
| [<span data-ttu-id="833d2-128">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="833d2-128">Discriminators are read-only</span></span>](#read-only-discriminators)                                                                             | <span data-ttu-id="833d2-129">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-129">Low</span></span>        |
| [<span data-ttu-id="833d2-130">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="833d2-130">Provider-specific EF.Functions methods throw for InMemory provider</span></span>](#no-client-methods)                                              | <span data-ttu-id="833d2-131">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-131">Low</span></span>        |
| [<span data-ttu-id="833d2-132">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="833d2-132">IndexBuilder.HasName is now obsolete</span></span>](#index-obsolete)                                                                               | <span data-ttu-id="833d2-133">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-133">Low</span></span>        |
| [<span data-ttu-id="833d2-134">Добавлено средство для преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="833d2-134">A pluarlizer is now included for scaffolding reverse engineered models</span></span>](#pluralizer)                                                 | <span data-ttu-id="833d2-135">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-135">Low</span></span>        |
| [<span data-ttu-id="833d2-136">INavigationBase заменяет INavigation в некоторых API, чтобы поддерживать пропуск навигации</span><span class="sxs-lookup"><span data-stu-id="833d2-136">INavigationBase replaces INavigation in some APIs to support skip navigations</span></span>](#inavigationbase)                                     | <span data-ttu-id="833d2-137">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-137">Low</span></span>        |
| <span data-ttu-id="833d2-138">[Некоторые запросы с помощью коррелированной коллекции, которые также используют `Distinct` или `GroupBy`, больше не поддерживаются](#collection-distinct-groupby).</span><span class="sxs-lookup"><span data-stu-id="833d2-138">[Some queries with correlated collection that also use `Distinct` or `GroupBy` are no longer supported](#collection-distinct-groupby)</span></span> | <span data-ttu-id="833d2-139">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-139">Low</span></span>        |
| [<span data-ttu-id="833d2-140">Использование коллекции запрашиваемого типа в проекции не поддерживается</span><span class="sxs-lookup"><span data-stu-id="833d2-140">Using a collection of Queryable type in projection is not supported</span></span>](#queryable-projection)                                          | <span data-ttu-id="833d2-141">Низкий</span><span class="sxs-lookup"><span data-stu-id="833d2-141">Low</span></span>        |

## <a name="medium-impact-changes"></a><span data-ttu-id="833d2-142">Изменения средней степени влияния</span><span class="sxs-lookup"><span data-stu-id="833d2-142">Medium-impact changes</span></span>

<a name="required-dependent"></a>

### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a><span data-ttu-id="833d2-143">Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="833d2-143">Required on the navigation from principal to dependent has different semantics</span></span>

[<span data-ttu-id="833d2-144">Отслеживание вопроса № 17286</span><span class="sxs-lookup"><span data-stu-id="833d2-144">Tracking Issue #17286</span></span>](https://github.com/dotnet/efcore/issues/17286)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-145">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-145">Old behavior</span></span>

<span data-ttu-id="833d2-146">При необходимости можно настроить только навигацию к основной сущности.</span><span class="sxs-lookup"><span data-stu-id="833d2-146">Only the navigations to principal could be configured as required.</span></span> <span data-ttu-id="833d2-147">Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="833d2-147">Therefore using `RequiredAttribute` on the navigation to the dependent (the entity containing the foreign key) would instead create the foreign key on the defining entity type.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-148">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-148">New behavior</span></span>

<span data-ttu-id="833d2-149">Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="833d2-149">With the added support for required dependents, it is now possible to mark any reference navigation as required, meaning that in the case shown above the foreign key will be defined on the other side of the relationship and the properties won't be marked as required.</span></span>

<span data-ttu-id="833d2-150">Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.</span><span class="sxs-lookup"><span data-stu-id="833d2-150">Calling `IsRequired` before specifying the dependent end is now ambiguous:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

#### <a name="why"></a><span data-ttu-id="833d2-151">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-151">Why</span></span>

<span data-ttu-id="833d2-152">Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).</span><span class="sxs-lookup"><span data-stu-id="833d2-152">The new behavior is necessary to enable support for required dependents ([see #12100](https://github.com/dotnet/efcore/issues/12100)).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-153">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-153">Mitigations</span></span>

<span data-ttu-id="833d2-154">Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="833d2-154">Remove `RequiredAttribute` from the navigation to the dependent and place it instead on the navigation to the principal or configure the relationship in `OnModelCreating`:</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a><span data-ttu-id="833d2-155">Определение запроса заменено методами, зависящими от поставщика</span><span class="sxs-lookup"><span data-stu-id="833d2-155">Defining query is replaced with provider-specific methods</span></span>

[<span data-ttu-id="833d2-156">Отслеживание вопроса № 18903</span><span class="sxs-lookup"><span data-stu-id="833d2-156">Tracking Issue #18903</span></span>](https://github.com/dotnet/efcore/issues/18903)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-157">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-157">Old behavior</span></span>

<span data-ttu-id="833d2-158">Типы сущностей сопоставлялись с определяющими запросами на уровне ядра.</span><span class="sxs-lookup"><span data-stu-id="833d2-158">Entity types were mapped to defining queries at the Core level.</span></span> <span data-ttu-id="833d2-159">Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.</span><span class="sxs-lookup"><span data-stu-id="833d2-159">Anytime the entity type was used in the query root of the entity type was replaced by the defining query for any provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-160">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-160">New behavior</span></span>

<span data-ttu-id="833d2-161">Интерфейсы API для определяющего запроса стали нерекомендуемыми.</span><span class="sxs-lookup"><span data-stu-id="833d2-161">APIs for defining query are deprecated.</span></span> <span data-ttu-id="833d2-162">Появились новые интерфейсы API для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="833d2-162">New provider-specific APIs were introduced.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-163">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-163">Why</span></span>

<span data-ttu-id="833d2-164">Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.</span><span class="sxs-lookup"><span data-stu-id="833d2-164">While defining queries were implemented as replacement query whenever query root is used in the query, it had a few issues:</span></span>

- <span data-ttu-id="833d2-165">Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.</span><span class="sxs-lookup"><span data-stu-id="833d2-165">If defining query is projecting entity type using `new { ... }` in `Select` method, then identifying that as an entity required additional work and made it inconsistent with how EF Core treats nominal types in the query.</span></span>
- <span data-ttu-id="833d2-166">Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.</span><span class="sxs-lookup"><span data-stu-id="833d2-166">For relational providers `FromSql` is still needed to pass the SQL string in LINQ expression form.</span></span>

<span data-ttu-id="833d2-167">Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных).</span><span class="sxs-lookup"><span data-stu-id="833d2-167">Initially defining queries were introduced as client-side views to be used with In-Memory provider for keyless entities (similar to database views in relational databases).</span></span> <span data-ttu-id="833d2-168">Такое определение упрощает тестирование приложения с базой данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="833d2-168">Such definition makes it easy to test application against in-memory database.</span></span> <span data-ttu-id="833d2-169">Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание.</span><span class="sxs-lookup"><span data-stu-id="833d2-169">Afterwards they became broadly applicable, which was useful but brought inconsistent and hard to understand behavior.</span></span> <span data-ttu-id="833d2-170">Поэтому мы решили упростить принцип их работы.</span><span class="sxs-lookup"><span data-stu-id="833d2-170">So we decided to simplify the concept.</span></span> <span data-ttu-id="833d2-171">Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому.</span><span class="sxs-lookup"><span data-stu-id="833d2-171">We made LINQ based defining query exclusive to In-Memory provider and treat them differently.</span></span> <span data-ttu-id="833d2-172">Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).</span><span class="sxs-lookup"><span data-stu-id="833d2-172">For more information, [see this issue](https://github.com/dotnet/efcore/issues/20023).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-173">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-173">Mitigations</span></span>

<span data-ttu-id="833d2-174">Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="833d2-174">For relational providers, use `ToSqlQuery` method in `OnModelCreating` and pass in a SQL string to use for the entity type.</span></span>
<span data-ttu-id="833d2-175">Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="833d2-175">For the In-Memory provider, use `ToInMemoryQuery` method in `OnModelCreating` and pass in a LINQ query to use for the entity type.</span></span>

<a name="nonnullreferences"></a>

### <a name="non-null-reference-navigations-are-not-overwritten-by-queries"></a><span data-ttu-id="833d2-176">Навигации по ссылке, которая не является пустой, не переопределяются запросами</span><span class="sxs-lookup"><span data-stu-id="833d2-176">Non-null reference navigations are not overwritten by queries</span></span>

[<span data-ttu-id="833d2-177">Отслеживание проблемы № 2693</span><span class="sxs-lookup"><span data-stu-id="833d2-177">Tracking Issue #2693</span></span>](https://github.com/dotnet/EntityFramework.Docs/issues/2693)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-178">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-178">Old behavior</span></span>

<span data-ttu-id="833d2-179">В EF Core 3.1 навигации по ссылкам, которые изначально инициализированы значениями, отличными от NULL, иногда перезаписывались экземплярами сущностей из базы данных, независимо от того, совпадали ли значения ключей.</span><span class="sxs-lookup"><span data-stu-id="833d2-179">In EF Core 3.1, reference navigations eagerly initialized to non-null values would sometimes be overwritten by entity instances from the database, regardless of whether or not key values matched.</span></span> <span data-ttu-id="833d2-180">Однако в других случаях в EF Core 3.1 выполнялось обратное действие и оставлялось существующее значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="833d2-180">However, in other cases, EF Core 3.1 would do the opposite and leave the existing non-null value.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-181">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-181">New behavior</span></span>

<span data-ttu-id="833d2-182">Начиная с версии EF Core 5.0, навигации по пустой ссылке, которая не является пустой, никогда не перезаписываются экземплярами, возвращаемыми запросом.</span><span class="sxs-lookup"><span data-stu-id="833d2-182">Starting with EF Core 5.0, non-null reference navigations are never overwritten by instances returned from a query.</span></span>

<span data-ttu-id="833d2-183">Обратите внимание, что безотложная инициализация навигации по _коллекции_ для пустой коллекции по-прежнему поддерживается.</span><span class="sxs-lookup"><span data-stu-id="833d2-183">Note that eager initialization of a _collection_ navigation to an empty collection is still supported.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-184">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-184">Why</span></span>

<span data-ttu-id="833d2-185">Инициализация свойства навигации по ссылкам для "пустого" экземпляра сущности приводит к неоднозначному состоянию.</span><span class="sxs-lookup"><span data-stu-id="833d2-185">Initialization of a reference navigation property to an "empty" entity instance results in an ambiguous state.</span></span> <span data-ttu-id="833d2-186">Пример:</span><span class="sxs-lookup"><span data-stu-id="833d2-186">For example:</span></span>

```csharp
public class Blog
{
     public int Id { get; set; }
     public Author Author { get; set; ) = new Author();
}
```

<span data-ttu-id="833d2-187">Обычно запрос блогов и авторов сначала создает экземпляры `Blog`, а затем задает соответствующие экземпляры `Author` на основе данных, возвращаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="833d2-187">Normally a query for Blogs and Authors will first create `Blog` instances and then set the appropriate `Author` instances based on the data returned from the database.</span></span> <span data-ttu-id="833d2-188">Однако в этом случае каждое свойство `Blog.Author` будет уже инициализировано для пустого свойства `Author`.</span><span class="sxs-lookup"><span data-stu-id="833d2-188">However, in this case every `Blog.Author` property is already initialized to an empty `Author`.</span></span> <span data-ttu-id="833d2-189">Кроме EF Core, не существует способа выяснить, является ли этот экземпляр "пустым".</span><span class="sxs-lookup"><span data-stu-id="833d2-189">Except EF Core has no way to know that this instance is "empty".</span></span> <span data-ttu-id="833d2-190">Таким образом, перезапись этого экземпляра может без уведомления привести к выводу допустимого `Author`.</span><span class="sxs-lookup"><span data-stu-id="833d2-190">So overwriting this instance could potentially silently throw away a valid `Author`.</span></span> <span data-ttu-id="833d2-191">Таким образом, EF Core 5.0 теперь не перезаписывает уже инициализированную навигацию.</span><span class="sxs-lookup"><span data-stu-id="833d2-191">Therefore, EF Core 5.0 now consistently does not overwrite a navigation that is already initialized.</span></span>

<span data-ttu-id="833d2-192">Это новое поведение также соответствует обычному поведению EF6, хотя при исследовании мы также обнаружили некоторые случаи несогласованности в EF6.</span><span class="sxs-lookup"><span data-stu-id="833d2-192">This new behavior also aligns with the behavior of EF6 in most cases, although upon investigation we also found some cases of inconsistency in EF6.</span></span>  

#### <a name="mitigations"></a><span data-ttu-id="833d2-193">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-193">Mitigations</span></span>

<span data-ttu-id="833d2-194">В случае обнаружения такого прерывания исправление будет способствовать остановке активной инициализации свойств навигации по ссылкам.</span><span class="sxs-lookup"><span data-stu-id="833d2-194">If this break is encountered, then the fix is to stop eagerly initializing reference navigation properties.</span></span>

## <a name="low-impact-changes"></a><span data-ttu-id="833d2-195">Изменения низкой степени влияния</span><span class="sxs-lookup"><span data-stu-id="833d2-195">Low-impact changes</span></span>

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="833d2-196">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="833d2-196">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="833d2-197">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="833d2-197">Tracking Issue #14257</span></span>](https://github.com/dotnet/efcore/issues/14257)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-198">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-198">Old behavior</span></span>

<span data-ttu-id="833d2-199">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="833d2-199">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="833d2-200">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="833d2-200">However, it only ever affected database creation.</span></span> <span data-ttu-id="833d2-201">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="833d2-201">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="833d2-202">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/dotnet/efcore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="833d2-202">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/dotnet/efcore/issues/14257)).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-203">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-203">New behavior</span></span>

<span data-ttu-id="833d2-204">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="833d2-204">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="833d2-205">Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="833d2-205">This API matches more closely to the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-206">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-206">Why</span></span>

<span data-ttu-id="833d2-207">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="833d2-207">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-208">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-208">Mitigations</span></span>

<span data-ttu-id="833d2-209">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="833d2-209">Use `HasColumnType` to specify the dimension:</span></span>

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

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a><span data-ttu-id="833d2-210">Cosmos: ключ секции теперь добавляется в первичный ключ</span><span class="sxs-lookup"><span data-stu-id="833d2-210">Cosmos: Partition key is now added to the primary key</span></span>

[<span data-ttu-id="833d2-211">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="833d2-211">Tracking Issue #15289</span></span>](https://github.com/dotnet/efcore/issues/15289)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-212">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-212">Old behavior</span></span>

<span data-ttu-id="833d2-213">Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.</span><span class="sxs-lookup"><span data-stu-id="833d2-213">The partition key property was only added to the alternate key that includes `id`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-214">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-214">New behavior</span></span>

<span data-ttu-id="833d2-215">В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="833d2-215">The partition key property is now also added to the primary key by convention.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-216">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-216">Why</span></span>

<span data-ttu-id="833d2-217">Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="833d2-217">This change makes the model better aligned with Azure Cosmos DB semantics and improves the performance of `Find` and some queries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-218">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-218">Mitigations</span></span>

<span data-ttu-id="833d2-219">Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="833d2-219">To prevent the partition key property to be added to the primary key, configure it in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a><span data-ttu-id="833d2-220">Cosmos: свойство `id` переименовано в `__id`</span><span class="sxs-lookup"><span data-stu-id="833d2-220">Cosmos: `id` property renamed to `__id`</span></span>

[<span data-ttu-id="833d2-221">Отслеживание вопроса № 17751</span><span class="sxs-lookup"><span data-stu-id="833d2-221">Tracking Issue #17751</span></span>](https://github.com/dotnet/efcore/issues/17751)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-222">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-222">Old behavior</span></span>

<span data-ttu-id="833d2-223">Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.</span><span class="sxs-lookup"><span data-stu-id="833d2-223">The shadow property mapped to the `id` JSON property was also named `id`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-224">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-224">New behavior</span></span>

<span data-ttu-id="833d2-225">Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.</span><span class="sxs-lookup"><span data-stu-id="833d2-225">The shadow property created by convention is now named `__id`.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-226">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-226">Why</span></span>

<span data-ttu-id="833d2-227">Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.</span><span class="sxs-lookup"><span data-stu-id="833d2-227">This change makes it less likely that the `id` property clashes with an existing property on the entity type.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-228">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-228">Mitigations</span></span>

<span data-ttu-id="833d2-229">Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="833d2-229">To go back to the 3.x behavior, configure the `id` property in `OnModelCreating`.</span></span>

```csharp
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a><span data-ttu-id="833d2-230">Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива</span><span class="sxs-lookup"><span data-stu-id="833d2-230">Cosmos: byte[] is now stored as a base64 string instead of a number array</span></span>

[<span data-ttu-id="833d2-231">Отслеживание вопроса № 17306</span><span class="sxs-lookup"><span data-stu-id="833d2-231">Tracking Issue #17306</span></span>](https://github.com/dotnet/efcore/issues/17306)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-232">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-232">Old behavior</span></span>

<span data-ttu-id="833d2-233">Свойства типа byte[] хранились в виде числового массива.</span><span class="sxs-lookup"><span data-stu-id="833d2-233">Properties of type byte[] were stored as a number array.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-234">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-234">New behavior</span></span>

<span data-ttu-id="833d2-235">Свойства типа byte[] теперь хранятся в виде строки base64.</span><span class="sxs-lookup"><span data-stu-id="833d2-235">Properties of type byte[] are now stored as a base64 string.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-236">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-236">Why</span></span>

<span data-ttu-id="833d2-237">Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.</span><span class="sxs-lookup"><span data-stu-id="833d2-237">This representation of byte[] aligns better with expectations and is the default behavior of the major JSON serialization libraries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-238">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-238">Mitigations</span></span>

<span data-ttu-id="833d2-239">Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки.</span><span class="sxs-lookup"><span data-stu-id="833d2-239">Existing data stored as number arrays will still be queried correctly, but currently there isn't a supported way to change back the insert behavior.</span></span> <span data-ttu-id="833d2-240">Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/dotnet/efcore/issues/17306)</span><span class="sxs-lookup"><span data-stu-id="833d2-240">If this limitation is blocking your scenario, comment on [this issue](https://github.com/dotnet/efcore/issues/17306)</span></span>

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a><span data-ttu-id="833d2-241">Cosmos: GetPropertyName и SetPropertyName переименованы</span><span class="sxs-lookup"><span data-stu-id="833d2-241">Cosmos: GetPropertyName and SetPropertyName were renamed</span></span>

[<span data-ttu-id="833d2-242">Отслеживание вопроса № 17874</span><span class="sxs-lookup"><span data-stu-id="833d2-242">Tracking Issue #17874</span></span>](https://github.com/dotnet/efcore/issues/17874)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-243">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-243">Old behavior</span></span>

<span data-ttu-id="833d2-244">Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="833d2-244">Previously the extension methods were called `GetPropertyName` and `SetPropertyName`</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-245">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-245">New behavior</span></span>

<span data-ttu-id="833d2-246">Старый интерфейс API был удален, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.</span><span class="sxs-lookup"><span data-stu-id="833d2-246">The old API was removed and new methods added: `GetJsonPropertyName`, `SetJsonPropertyName`</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-247">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-247">Why</span></span>

<span data-ttu-id="833d2-248">Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.</span><span class="sxs-lookup"><span data-stu-id="833d2-248">This change removes the ambiguity around what these methods are configuring.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-249">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-249">Mitigations</span></span>

<span data-ttu-id="833d2-250">Используйте новый API.</span><span class="sxs-lookup"><span data-stu-id="833d2-250">Use the new API.</span></span>

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a><span data-ttu-id="833d2-251">Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"</span><span class="sxs-lookup"><span data-stu-id="833d2-251">Value generators are called when the entity state is changed from Detached to Unchanged, Updated, or Deleted</span></span>

[<span data-ttu-id="833d2-252">Отслеживание вопроса № 15289</span><span class="sxs-lookup"><span data-stu-id="833d2-252">Tracking Issue #15289</span></span>](https://github.com/dotnet/efcore/issues/15289)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-253">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-253">Old behavior</span></span>

<span data-ttu-id="833d2-254">Генераторы значений вызывались только при изменении состояния сущности на «добавлено».</span><span class="sxs-lookup"><span data-stu-id="833d2-254">Value generators were only called when the entity state changed to Added.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-255">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-255">New behavior</span></span>

<span data-ttu-id="833d2-256">Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="833d2-256">Value generators are now called when the entity state is changed from Detached to Unchanged, Updated, or Deleted and the property contains the default values.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-257">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-257">Why</span></span>

<span data-ttu-id="833d2-258">Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.</span><span class="sxs-lookup"><span data-stu-id="833d2-258">This change was necessary to improve the experience with properties that are not persisted to the data store and have their value generated always on the client.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-259">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-259">Mitigations</span></span>

<span data-ttu-id="833d2-260">Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.</span><span class="sxs-lookup"><span data-stu-id="833d2-260">To prevent the value generator from being called, assign a non-default value to the property before the state is changed.</span></span>

<a name="relational-model"></a>

### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a><span data-ttu-id="833d2-261">IMigrationsModelDiffer теперь использует IRelationalModel</span><span class="sxs-lookup"><span data-stu-id="833d2-261">IMigrationsModelDiffer now uses IRelationalModel</span></span>

[<span data-ttu-id="833d2-262">Отслеживание вопроса № 20305</span><span class="sxs-lookup"><span data-stu-id="833d2-262">Tracking Issue #20305</span></span>](https://github.com/dotnet/efcore/issues/20305)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-263">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-263">Old behavior</span></span>

<span data-ttu-id="833d2-264">API `IMigrationsModelDiffer` определялся с помощью `IModel`.</span><span class="sxs-lookup"><span data-stu-id="833d2-264">`IMigrationsModelDiffer` API was defined using `IModel`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-265">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-265">New behavior</span></span>

<span data-ttu-id="833d2-266">Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`.</span><span class="sxs-lookup"><span data-stu-id="833d2-266">`IMigrationsModelDiffer` API now uses `IRelationalModel`.</span></span> <span data-ttu-id="833d2-267">Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.</span><span class="sxs-lookup"><span data-stu-id="833d2-267">However the model snapshot still contains only `IModel` as this code is part of the application and Entity Framework can't change it without making a bigger breaking change.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-268">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-268">Why</span></span>

<span data-ttu-id="833d2-269">`IRelationalModel` — это вновь добавленное представление схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="833d2-269">`IRelationalModel` is a newly added representation of the database schema.</span></span> <span data-ttu-id="833d2-270">Его использование для поиска различий выполняется быстрее и точнее.</span><span class="sxs-lookup"><span data-stu-id="833d2-270">Using it to find differences is faster and more accurate.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-271">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-271">Mitigations</span></span>

<span data-ttu-id="833d2-272">Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.</span><span class="sxs-lookup"><span data-stu-id="833d2-272">Use the following code to compare the model from `snapshot` with the model from `context`:</span></span>

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

<span data-ttu-id="833d2-273">Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).</span><span class="sxs-lookup"><span data-stu-id="833d2-273">We are planning to improve this experience in 6.0 ([see #22031](https://github.com/dotnet/efcore/issues/22031))</span></span>

<a name="read-only-discriminators"></a>

### <a name="discriminators-are-read-only"></a><span data-ttu-id="833d2-274">Дискриминаторы доступны только для чтения</span><span class="sxs-lookup"><span data-stu-id="833d2-274">Discriminators are read-only</span></span>

[<span data-ttu-id="833d2-275">Отслеживание вопроса № 21154</span><span class="sxs-lookup"><span data-stu-id="833d2-275">Tracking Issue #21154</span></span>](https://github.com/dotnet/efcore/issues/21154)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-276">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-276">Old behavior</span></span>

<span data-ttu-id="833d2-277">Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="833d2-277">It was possible to change the discriminator value before calling `SaveChanges`</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-278">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-278">New behavior</span></span>

<span data-ttu-id="833d2-279">В приведенном выше случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="833d2-279">An exception will be throws in the above case.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-280">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-280">Why</span></span>

<span data-ttu-id="833d2-281">EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="833d2-281">EF doesn't expect the entity type to change while it is still being tracked, so changing the discriminator value leaves the context in an inconsistent state, which might result in unexpected behavior.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-282">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-282">Mitigations</span></span>

<span data-ttu-id="833d2-283">Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.</span><span class="sxs-lookup"><span data-stu-id="833d2-283">If changing the discriminator value is necessary and the context will be disposed immediately after calling `SaveChanges`, the discriminator can be made mutable:</span></span>

```csharp
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```

<a name="no-client-methods"></a>

### <a name="provider-specific-effunctions-methods-throw-for-inmemory-provider"></a><span data-ttu-id="833d2-284">Относящиеся к поставщику методы EF.Functions вызываются для поставщика InMemory</span><span class="sxs-lookup"><span data-stu-id="833d2-284">Provider-specific EF.Functions methods throw for InMemory provider</span></span>

[<span data-ttu-id="833d2-285">Отслеживание вопроса № 20294</span><span class="sxs-lookup"><span data-stu-id="833d2-285">Tracking Issue #20294</span></span>](https://github.com/dotnet/efcore/issues/20294)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-286">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-286">Old behavior</span></span>

<span data-ttu-id="833d2-287">Относящиеся к конкретному поставщику методы EF.Functions содержали реализацию для выполнения на стороне клиента, что позволяло им выполняться в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="833d2-287">Provider-specific EF.Functions methods contained implementation for client execution, which allowed them to be executed on the InMemory provider.</span></span> <span data-ttu-id="833d2-288">Например, `EF.Functions.DateDiffDay` — это относящийся к SQL Server метод, который работал в поставщике InMemory.</span><span class="sxs-lookup"><span data-stu-id="833d2-288">For example, `EF.Functions.DateDiffDay` is a Sql Server specific method, which worked on InMemory provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-289">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-289">New behavior</span></span>

<span data-ttu-id="833d2-290">Методы, относящиеся к конкретному поставщику, были изменены так, чтобы в теле метода создавалось исключение, блокирующее вычисление метода на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="833d2-290">Provider-specific methods have been updated to throw an exception in their method body to block evaluating them on client side.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-291">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-291">Why</span></span>

<span data-ttu-id="833d2-292">Методы, относящиеся к конкретному поставщику, сопоставляются с функцией базы данных.</span><span class="sxs-lookup"><span data-stu-id="833d2-292">Provider-specific methods map to a database function.</span></span> <span data-ttu-id="833d2-293">Вычисление, выполняемое сопоставленной функцией базы данных, не всегда может реплицироваться на стороне клиента в LINQ.</span><span class="sxs-lookup"><span data-stu-id="833d2-293">The computation done by the mapped database function can't always be replicated on the client side in LINQ.</span></span> <span data-ttu-id="833d2-294">Поэтому результаты, полученные при выполнении одного и того же метода на сервере и в клиенте, могут различаться.</span><span class="sxs-lookup"><span data-stu-id="833d2-294">It may cause the result from the server to differ when executing the same method on client.</span></span> <span data-ttu-id="833d2-295">Так как эти методы используются в LINQ для преобразования в определенные функции базы данных, их не нужно вычислять на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="833d2-295">Since these methods are used in LINQ to translate to specific database functions, they don't need to be evaluated on client side.</span></span> <span data-ttu-id="833d2-296">Так как поставщик InMemory — это другая *база данных*, эти методы недоступны для него.</span><span class="sxs-lookup"><span data-stu-id="833d2-296">As InMemory provider is a different *database*, these methods aren't available for this provider.</span></span> <span data-ttu-id="833d2-297">При попытке выполнить их для поставщика InMemory или любого другого поставщика, который не преобразует эти методы, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="833d2-297">Trying to execute them for InMemory provider, or any other provider that doesn't translate these methods, throws an exception.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-298">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-298">Mitigations</span></span>

<span data-ttu-id="833d2-299">Так как точно воссоздать поведение функций базы данных невозможно, содержащие их запросы следует тестировать на основе базы данных того же типа, что и рабочая база данных.</span><span class="sxs-lookup"><span data-stu-id="833d2-299">Since there's no way to mimic behavior of database functions accurately, you should test the queries containing them against same kind of database as in production.</span></span>

<a name="index-obsolete"></a>

### <a name="indexbuilderhasname-is-now-obsolete"></a><span data-ttu-id="833d2-300">IndexBuilder.HasName считается устаревшим</span><span class="sxs-lookup"><span data-stu-id="833d2-300">IndexBuilder.HasName is now obsolete</span></span>

[<span data-ttu-id="833d2-301">Отслеживание проблемы № 21089</span><span class="sxs-lookup"><span data-stu-id="833d2-301">Tracking Issue #21089</span></span>](https://github.com/dotnet/efcore/issues/21089)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-302">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-302">Old behavior</span></span>

<span data-ttu-id="833d2-303">Ранее допускалось определение только одного индекса для конкретного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="833d2-303">Previously, only one index could be defined over a given set of properties.</span></span> <span data-ttu-id="833d2-304">Имя для базы данных индекса настраивалось с использованием IndexBuilder.HasName.</span><span class="sxs-lookup"><span data-stu-id="833d2-304">The database name of an index was configured using IndexBuilder.HasName.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-305">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-305">New behavior</span></span>

<span data-ttu-id="833d2-306">Теперь допускается несколько индексов для одного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="833d2-306">Multiple indexes are now allowed on the same set or properties.</span></span> <span data-ttu-id="833d2-307">Эти индексы отличаются именами в пределах модели.</span><span class="sxs-lookup"><span data-stu-id="833d2-307">These indexes are now distinguished by a name in the model.</span></span> <span data-ttu-id="833d2-308">По соглашению имя модели используется в качестве имени базы данных, но его также можно указать независимо с использованием HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="833d2-308">By convention, the model name is used as the database name; however it can also be configured independently using HasDatabaseName.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-309">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-309">Why</span></span>

<span data-ttu-id="833d2-310">В будущем мы хотим включить для набора свойств параллельные индексы с сортировкой по возрастанию и убыванию, а также индексы с разными параметрами сортировки.</span><span class="sxs-lookup"><span data-stu-id="833d2-310">In the future, we'd like to enable both ascending and descending indexes or indexes with different collations on the same set of properties.</span></span> <span data-ttu-id="833d2-311">Это изменение продвигает нас в этом направлении.</span><span class="sxs-lookup"><span data-stu-id="833d2-311">This change moves us another step in that direction.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-312">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-312">Mitigations</span></span>

<span data-ttu-id="833d2-313">Любой код, который ранее вызывал IndexBuilder.HasName, необходимо изменить для вызова HasDatabaseName.</span><span class="sxs-lookup"><span data-stu-id="833d2-313">Any code that was previously calling IndexBuilder.HasName should be updated to call HasDatabaseName instead.</span></span>

<span data-ttu-id="833d2-314">Если в проекте есть миграции, созданные до версии EF Core 2.0.0, вы можете игнорировать предупреждения для этих файлов, подавив их с помощью `#pragma warning disable 612, 618`.</span><span class="sxs-lookup"><span data-stu-id="833d2-314">If your project includes migrations generated prior to EF Core version 2.0.0, you can safely ignore the warning in those files and suppress it by adding `#pragma warning disable 612, 618`.</span></span>

<a name="pluralizer"></a>

### <a name="a-pluralizer-is-now-included-for-scaffolding-reverse-engineered-models"></a><span data-ttu-id="833d2-315">Добавлено средство преобразования во множественное число для формирования шаблонов реконструированных моделей</span><span class="sxs-lookup"><span data-stu-id="833d2-315">A pluralizer is now included for scaffolding reverse engineered models</span></span>

[<span data-ttu-id="833d2-316">Отслеживание проблемы № 11160</span><span class="sxs-lookup"><span data-stu-id="833d2-316">Tracking Issue #11160</span></span>](https://github.com/dotnet/efcore/issues/11160)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-317">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-317">Old behavior</span></span>

<span data-ttu-id="833d2-318">Ранее нужно было устанавливать отдельный пакет, чтобы преобразовывать имена DbSet и коллекций во множественное число и имена таблиц в единственное число при создании шаблонов DbContext и типов сущностей путем реконструирования из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="833d2-318">Previously, you had to install a separate pluralizer package in order to pluralize DbSet and collection navigation names and singularize table names when scaffolding a DbContext and entity types by reverse engineering a database schema.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-319">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-319">New behavior</span></span>

<span data-ttu-id="833d2-320">Теперь EF Core содержит собственное средство для преобразования во множественное число на основе библиотеки [Humanizer](https://github.com/Humanizr/Humanizer).</span><span class="sxs-lookup"><span data-stu-id="833d2-320">EF Core now includes a pluralizer that uses the [Humanizer](https://github.com/Humanizr/Humanizer) library.</span></span> <span data-ttu-id="833d2-321">Именно эту библиотеку использует Visual Studio, рекомендуя имена переменных.</span><span class="sxs-lookup"><span data-stu-id="833d2-321">This is the same library Visual Studio uses to recommend variable names.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-322">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-322">Why</span></span>

<span data-ttu-id="833d2-323">Использование форм множественного числа для свойств коллекций и единственного числа для типов и ссылочных свойств считается идиоматичным в .NET.</span><span class="sxs-lookup"><span data-stu-id="833d2-323">Using plural forms of words for collection properties and singular forms for types and reference properties is idiomatic in .NET.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-324">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-324">Mitigations</span></span>

<span data-ttu-id="833d2-325">Чтобы отключить средство для преобразования во множественное число, укажите параметр `--no-pluralize` для `dotnet ef dbcontext scaffold` или `-NoPluralize` для `Scaffold-DbContext`.</span><span class="sxs-lookup"><span data-stu-id="833d2-325">To disable the pluralizer, use the `--no-pluralize` option on `dotnet ef dbcontext scaffold` or the `-NoPluralize` switch on `Scaffold-DbContext`.</span></span>

<a name="inavigationbase"></a>

### <a name="inavigationbase-replaces-inavigation-in-some-apis-to-support-skip-navigations"></a><span data-ttu-id="833d2-326">INavigationBase заменяет INavigation в некоторых API, чтобы поддерживать пропуск навигации</span><span class="sxs-lookup"><span data-stu-id="833d2-326">INavigationBase replaces INavigation in some APIs to support skip navigations</span></span>

[<span data-ttu-id="833d2-327">Отслеживание проблемы № 2568</span><span class="sxs-lookup"><span data-stu-id="833d2-327">Tracking Issue #2568</span></span>](https://github.com/dotnet/EntityFramework.Docs/issues/2568)

#### <a name="old-behavior"></a><span data-ttu-id="833d2-328">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-328">Old behavior</span></span>

<span data-ttu-id="833d2-329">В версиях EF Core, предшествующих версии 5.0, поддерживалась только одна форма свойства навигации, представленная интерфейсом `INavigation`.</span><span class="sxs-lookup"><span data-stu-id="833d2-329">EF Core prior to 5.0 supported only one form of navigation property, represented by the `INavigation` interface.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="833d2-330">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="833d2-330">New behavior</span></span>

<span data-ttu-id="833d2-331">EF Core 5.0 представляет связи "многие ко многим", которые используют "пропуск навигации".</span><span class="sxs-lookup"><span data-stu-id="833d2-331">EF Core 5.0 introduces many-to-many relationships which use "skip navigations".</span></span> <span data-ttu-id="833d2-332">Они представлены интерфейсом `ISkipNavigation`, а большинство функциональных возможностей `INavigation` переданы в общий базовый интерфейс: `INavigationBase`.</span><span class="sxs-lookup"><span data-stu-id="833d2-332">These are represented by the `ISkipNavigation` interface, and most of the functionality of `INavigation` has been pushed down to a common base interface: `INavigationBase`.</span></span>

#### <a name="why"></a><span data-ttu-id="833d2-333">Почему</span><span class="sxs-lookup"><span data-stu-id="833d2-333">Why</span></span>

<span data-ttu-id="833d2-334">Большая часть функций между обычной навигацией и пропуском навигации одинаковы.</span><span class="sxs-lookup"><span data-stu-id="833d2-334">Most of the functionality between normal and skip navigations is the same.</span></span> <span data-ttu-id="833d2-335">Тем не менее пропуск навигации имеет другую связь с внешними ключами по сравнению с обычной навигацией, поскольку вовлеченные внешние ключи находятся не на обоих концах связи, а скорее в объекте соединения.</span><span class="sxs-lookup"><span data-stu-id="833d2-335">However, skip navigations have a different relationship to foreign keys than normal navigations, since the FKs involved are not directly on either end of the relationship, but rather in the join entity.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="833d2-336">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="833d2-336">Mitigations</span></span>

<span data-ttu-id="833d2-337">Во многих случаях приложения могут переключиться на использование нового базового интерфейса, не нуждаясь в дополнительных изменениях.</span><span class="sxs-lookup"><span data-stu-id="833d2-337">In many cases applications can switch to using the new base interface with no other changes.</span></span> <span data-ttu-id="833d2-338">Однако если навигация используется для доступа к свойствам внешнего ключа, код приложения нужно либо ограничить только обычными переходами, либо обновить, чтобы он мог выполнять соответствующие действия как для обычной, так и для пропускной навигации.</span><span class="sxs-lookup"><span data-stu-id="833d2-338">However, in cases where the navigation is used to access foreign key properties, application code should either be constrained to only normal navigations, or updated to do the appropriate thing for both normal and skip navigations.</span></span>

<a name="collection-distinct-groupby"></a>

### <a name="some-queries-with-correlated-collection-that-also-use-distinct-or-groupby-are-no-longer-supported"></a><span data-ttu-id="833d2-339">Некоторые запросы с помощью коррелированной коллекции, которые также используют `Distinct` или `GroupBy`, больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="833d2-339">Some queries with correlated collection that also use `Distinct` or `GroupBy` are no longer supported</span></span>

[<span data-ttu-id="833d2-340">Отслеживание проблемы № 15873</span><span class="sxs-lookup"><span data-stu-id="833d2-340">Tracking Issue #15873</span></span>](https://github.com/dotnet/efcore/issues/15873)

<span data-ttu-id="833d2-341">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="833d2-341">**Old behavior**</span></span>

<span data-ttu-id="833d2-342">Ранее мы позволяли выполнять запросы, включающие коррелированные коллекции с последующим свойством `GroupBy` и некоторые запросы с использованием `Distinct`.</span><span class="sxs-lookup"><span data-stu-id="833d2-342">Previously, queries involving correlated collections followed by `GroupBy`, as well as some queries using `Distinct` we allowed to execute.</span></span>

<span data-ttu-id="833d2-343">Пример GroupBy:</span><span class="sxs-lookup"><span data-stu-id="833d2-343">GroupBy example:</span></span>

```csharp
context.Parents
    .Select(p => p.Children
        .GroupBy(c => c.School)
        .Select(g => g.Key))
```

<span data-ttu-id="833d2-344">Пример `Distinct`, в частности запросы `Distinct`, в которых внутренняя проекция коллекции не содержит первичный ключ:</span><span class="sxs-lookup"><span data-stu-id="833d2-344">`Distinct` example - specifically `Distinct` queries where inner collection projection doesn't contain the primary key:</span></span>

```csharp
context.Parents
    .Select(p => p.Children
        .Select(c => c.School)
        .Distinct())
```

<span data-ttu-id="833d2-345">Эти запросы могли возвращать неверные результаты, если внутренняя коллекция содержала дубликаты, однако работала должным образом, если все элементы во внутренней коллекции являлись уникальными.</span><span class="sxs-lookup"><span data-stu-id="833d2-345">These queries could return incorrect results if the inner collection contained any duplicates, but worked correctly if all the elements in the inner collection were unique.</span></span>

<span data-ttu-id="833d2-346">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="833d2-346">**New behavior**</span></span>

<span data-ttu-id="833d2-347">Эти запросы больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="833d2-347">These queries are no longer supported.</span></span> <span data-ttu-id="833d2-348">Создается исключение, указывающее, что у нас недостаточно сведений для правильного создания результатов.</span><span class="sxs-lookup"><span data-stu-id="833d2-348">Exception is thrown indicating that we don't have enough information to correctly build the results.</span></span>

<span data-ttu-id="833d2-349">**Причина**</span><span class="sxs-lookup"><span data-stu-id="833d2-349">**Why**</span></span>

<span data-ttu-id="833d2-350">В сценариях с коррелированными коллекциями, чтобы назначить сущность коллекции правильному родительскому объекту, нужно знать первичный ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="833d2-350">For correlated collection scenarios we need to know entity's primary key in order to assign collection entities to the correct parent.</span></span> <span data-ttu-id="833d2-351">Если внутренняя коллекция не использует `GroupBy` или `Distinct`, отсутствующий первичный ключ можно просто добавить в проекцию.</span><span class="sxs-lookup"><span data-stu-id="833d2-351">When inner collection doesn't use `GroupBy` or `Distinct`, the missing primary key can simply be added to the projection.</span></span> <span data-ttu-id="833d2-352">Однако при использовании `GroupBy` и `Distinct` это не удастся сделать, так как такое действие приведет к изменению результата операции `GroupBy` или `Distinct`.</span><span class="sxs-lookup"><span data-stu-id="833d2-352">However in case of `GroupBy` and `Distinct` it can't be done because it would change the result of `GroupBy` or `Distinct` operation.</span></span>

<span data-ttu-id="833d2-353">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="833d2-353">**Mitigations**</span></span>

<span data-ttu-id="833d2-354">Перепишите запрос, чтобы не использовать операции `GroupBy` или `Distinct` во внутренней коллекции, и выполните эти операции на клиенте.</span><span class="sxs-lookup"><span data-stu-id="833d2-354">Rewrite the query to not use `GroupBy` or `Distinct` operations on the inner collection, and perform these operations on the client instead.</span></span>

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

### <a name="using-a-collection-of-queryable-type-in-projection-is-not-supported"></a><span data-ttu-id="833d2-355">Использование коллекции запрашиваемого типа в проекции не поддерживается</span><span class="sxs-lookup"><span data-stu-id="833d2-355">Using a collection of Queryable type in projection is not supported</span></span>

[<span data-ttu-id="833d2-356">Отслеживание проблемы № 16314</span><span class="sxs-lookup"><span data-stu-id="833d2-356">Tracking Issue #16314</span></span>](https://github.com/dotnet/efcore/issues/16314)

<span data-ttu-id="833d2-357">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="833d2-357">**Old behavior**</span></span>

<span data-ttu-id="833d2-358">Ранее в отдельных случаях коллекцию запрашиваемых типов можно было использовать в проекции, например как аргумент для конструктора `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="833d2-358">Previously, it was possible to use collection of a Queryable type inside the projection in some cases, for example as an argument to a `List<T>` constructor:</span></span>

```csharp
context.Blogs
    .Select(b => new List<Post>(context.Posts.Where(p => p.BlogId == b.Id)))
```

<span data-ttu-id="833d2-359">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="833d2-359">**New behavior**</span></span>

<span data-ttu-id="833d2-360">Эти запросы больше не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="833d2-360">These queries are no longer supported.</span></span> <span data-ttu-id="833d2-361">Создается исключение с указанием на невозможность создания объекта запрашиваемого типа и предложением того, как это можно исправить.</span><span class="sxs-lookup"><span data-stu-id="833d2-361">Exception is thrown indicating that we can't create an object of Queryable type and suggesting how this could be fixed.</span></span>

<span data-ttu-id="833d2-362">**Причина**</span><span class="sxs-lookup"><span data-stu-id="833d2-362">**Why**</span></span>

<span data-ttu-id="833d2-363">Мы не можем реализовать объект запрашиваемого типа, поэтому такие объекты автоматически создавались с помощью типа `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="833d2-363">We can't materialize an object of a Queryable type, so they would automatically be created using `List<T>` type instead.</span></span> <span data-ttu-id="833d2-364">Это часто приводило к выводу исключения из-за несоответствия типов, которое было непонятным и могло быть неожиданным для некоторых пользователей.</span><span class="sxs-lookup"><span data-stu-id="833d2-364">This would often cause an exception due to type mismatch which was not very clear and could be surprising to some users.</span></span> <span data-ttu-id="833d2-365">Мы решили распознать шаблон и выдать более осмысленное исключение.</span><span class="sxs-lookup"><span data-stu-id="833d2-365">We decided to recognize the pattern and throw a more meaningful exception.</span></span>

<span data-ttu-id="833d2-366">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="833d2-366">**Mitigations**</span></span>

<span data-ttu-id="833d2-367">Добавьте вызов `ToList()` после запрашиваемого объекта в проекции:</span><span class="sxs-lookup"><span data-stu-id="833d2-367">Add `ToList()` call after the Queryable object in the projection:</span></span>

```csharp
context.Blogs.Select(b => context.Posts.Where(p => p.BlogId == b.Id).ToList())
```
