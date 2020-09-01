---
title: Планирование для Entity Framework Core 5.0
author: ajcvickers
ms.date: 08/22/2020
uid: core/what-is-new/ef-core-5.0/plan
ms.openlocfilehash: a0d41d6df844c9ca2c8a2dc8ba50ca669e23dced
ms.sourcegitcommit: 6f7af3f138bf7c724cbdda261f97e5cf7035e8d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2020
ms.locfileid: "88847583"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="19711-102">Планирование для Entity Framework Core 5.0</span><span class="sxs-lookup"><span data-stu-id="19711-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="19711-103">Как было описано в [процессе планирования](xref:core/what-is-new/release_planning), мы собрали сведения от заинтересованных лиц в предварительный план для выпуска EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-103">As described in the [planning process](xref:core/what-is-new/release_planning), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19711-104">Этот план отражает все еще продолжающиеся работы.</span><span class="sxs-lookup"><span data-stu-id="19711-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="19711-105">Ничего из приведенного здесь не является обязательством.</span><span class="sxs-lookup"><span data-stu-id="19711-105">Nothing here is a commitment.</span></span> <span data-ttu-id="19711-106">Этот план является отправной точкой и будет расширяться по мере обучения.</span><span class="sxs-lookup"><span data-stu-id="19711-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="19711-107">Могут быть включены некоторые вещи, пока не запланированные для версии 5.0,</span><span class="sxs-lookup"><span data-stu-id="19711-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="19711-108">и исключены некоторые вещи, уже запланированные для этой версии.</span><span class="sxs-lookup"><span data-stu-id="19711-108">Some things currently planned for 5.0 may get punted out.</span></span>

## <a name="general-information"></a><span data-ttu-id="19711-109">Общие сведения</span><span class="sxs-lookup"><span data-stu-id="19711-109">General information</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="19711-110">Номер версии и дата выпуска</span><span class="sxs-lookup"><span data-stu-id="19711-110">Version number and release date</span></span>

<span data-ttu-id="19711-111">Сейчас выпуск EF Core 5.0 запланирован на [то же самое время, что и.NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="19711-111">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="19711-112">Версия "5.0" была выбрана для согласования с .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-112">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="19711-113">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="19711-113">Supported platforms</span></span>

<span data-ttu-id="19711-114">Планируется, что EF Core 5.0 будет работать на любой платформе .NET Standard 2.1, включая .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-114">EF Core 5.0 is planned to run on any .NET Standard 2.1 platform, including .NET 5.0.</span></span> <span data-ttu-id="19711-115">Это согласуется с более общей стратегией [конвергенции платформ .NET в .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="19711-115">This is part of the more general .NET wide [convergence of platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span>

<span data-ttu-id="19711-116">EF Core 5.0 не будет работать в .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="19711-116">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="19711-117">Критические изменения</span><span class="sxs-lookup"><span data-stu-id="19711-117">Breaking changes</span></span>

<span data-ttu-id="19711-118">EF Core 5.0 будет содержать некоторые [критические изменения](xref:core/what-is-new/ef-core-5.0/breaking-changes), но они будут гораздо менее серьезными, чем в случае с EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="19711-118">EF Core 5.0 will contain some [breaking changes](xref:core/what-is-new/ef-core-5.0/breaking-changes), but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="19711-119">Наша цель состоит в том, чтобы позволить подавляющему большинству приложений осуществлять обновление без нарушения работы.</span><span class="sxs-lookup"><span data-stu-id="19711-119">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="19711-120">Ожидаются некоторые критические изменения для поставщиков баз данных, особенно связанные с поддержкой метода "одна таблица на тип".</span><span class="sxs-lookup"><span data-stu-id="19711-120">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="19711-121">Однако мы ожидаем, что обновление поставщика для версии 5.0 потребует меньше работы, чем при обновлении для версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="19711-121">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="19711-122">Темы</span><span class="sxs-lookup"><span data-stu-id="19711-122">Themes</span></span>

<span data-ttu-id="19711-123">Мы определили несколько основных областей или тем, которые сформируют основу для крупномасштабных инвестиций в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-123">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="fully-transparent-many-to-many-mapping-by-convention"></a><span data-ttu-id="19711-124">Полностью прозрачное сопоставление "многие ко многим" в соответствии с соглашением</span><span class="sxs-lookup"><span data-stu-id="19711-124">Fully transparent many-to-many mapping by convention</span></span>

<span data-ttu-id="19711-125">Ведущие разработчики: @smitpatel, @AndriySvyryd и @lajones</span><span class="sxs-lookup"><span data-stu-id="19711-125">Lead developers: @smitpatel, @AndriySvyryd, and @lajones</span></span>

<span data-ttu-id="19711-126">Отслеживается по проблеме [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)</span><span class="sxs-lookup"><span data-stu-id="19711-126">Tracked by [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)</span></span>

<span data-ttu-id="19711-127">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-127">T-shirt size: L</span></span>

<span data-ttu-id="19711-128">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-128">Status: Done</span></span>

<span data-ttu-id="19711-129">Функция "многие ко многим" является [наиболее востребованной](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (около 506 голосов) в журнале невыполненной работы GitHub.</span><span class="sxs-lookup"><span data-stu-id="19711-129">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~506 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="19711-130">Поддержку связей "многие ко многим" можно разбить на три основные области:</span><span class="sxs-lookup"><span data-stu-id="19711-130">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="19711-131">Пропуск свойств навигации — рассматривается в следующей теме.</span><span class="sxs-lookup"><span data-stu-id="19711-131">Skip navigation properties--covered by the next theme.</span></span>
* <span data-ttu-id="19711-132">Типы сущностей контейнера свойств.</span><span class="sxs-lookup"><span data-stu-id="19711-132">Property-bag entity types.</span></span> <span data-ttu-id="19711-133">Они позволяют использовать стандартный тип CLR (например, `Dictionary`) для экземпляров сущностей таким образом, чтобы для каждого типа сущности не требовался явный тип CLR.</span><span class="sxs-lookup"><span data-stu-id="19711-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="19711-134">Отслеживается по проблеме [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).</span><span class="sxs-lookup"><span data-stu-id="19711-134">Tracked by [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).</span></span>
* <span data-ttu-id="19711-135">Средства для простоты настройки связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="19711-135">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="19711-136">Помимо поддержки пропуска навигации, эти возможности связи "многие ко многим" перемещаются в EF Core 5.0 для обеспечения эффективной работы.</span><span class="sxs-lookup"><span data-stu-id="19711-136">In addition to the skip navigation support, we are now pulling these other areas of many-to-many into EF Core 5.0 so as to provide a complete experience.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="19711-137">Свойства навигации "многие ко многим" (или "навигации с пропуском")</span><span class="sxs-lookup"><span data-stu-id="19711-137">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="19711-138">Ведущие разработчики: @smitpatel и @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="19711-138">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="19711-139">Отслеживание с помощью [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="19711-139">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="19711-140">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-140">T-shirt size: L</span></span>

<span data-ttu-id="19711-141">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-141">Status: Done</span></span>

<span data-ttu-id="19711-142">Как описано в первой теме, поддержка "многие ко многим" имеет несколько аспектов.</span><span class="sxs-lookup"><span data-stu-id="19711-142">As described in the first theme, many-to-many support has multiple aspects.</span></span>
<span data-ttu-id="19711-143">В этой теме, в частности, отслеживается использование пропуска навигаций.</span><span class="sxs-lookup"><span data-stu-id="19711-143">This theme specifically tracks use of skip navigations.</span></span>
<span data-ttu-id="19711-144">Мы считаем, что наиболее существенным препятствием для тех, кому требуется поддержка типа "многие ко многим", заключается в невозможности использовать "естественные" связи, не ссылаясь на таблицу соединений, в бизнес-логике, такой как запросы.</span><span class="sxs-lookup"><span data-stu-id="19711-144">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span>
<span data-ttu-id="19711-145">Тип сущности таблицы соединений может по-прежнему существовать, но не должен мешать бизнес-логике.</span><span class="sxs-lookup"><span data-stu-id="19711-145">The join table entity type may still exist, but it should not get in the way of business logic.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="19711-146">Сопоставление наследования "одна таблица на тип"</span><span class="sxs-lookup"><span data-stu-id="19711-146">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="19711-147">Ведущий разработчик: @AndriySvyryd и @smitpatel</span><span class="sxs-lookup"><span data-stu-id="19711-147">Lead developer: @AndriySvyryd and @smitpatel</span></span>

<span data-ttu-id="19711-148">Отслеживание с помощью [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="19711-148">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="19711-149">Размер футболки: XL</span><span class="sxs-lookup"><span data-stu-id="19711-149">T-shirt size: XL</span></span>

<span data-ttu-id="19711-150">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-150">Status: Done</span></span>

<span data-ttu-id="19711-151">Мы работаем над методом "одна таблица на тип", так как это очень востребованная функция (около 289 голосов; третье место) и для нее требуются некоторые низкоуровневые изменения, которые, по нашему мнению, согласуются с основополагающим характером общего плана по .NET 5.</span><span class="sxs-lookup"><span data-stu-id="19711-151">We're doing TPT because it is both a highly requested feature (~289 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="19711-152">Мы предполагаем, что это приведет к критическим изменениям для поставщиков баз данных, хотя они должны быть гораздо менее серьезными, чем для версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="19711-152">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="19711-153">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="19711-153">Filtered Include</span></span>

<span data-ttu-id="19711-154">Ведущий разработчик: @maumar</span><span class="sxs-lookup"><span data-stu-id="19711-154">Lead developer: @maumar</span></span>

<span data-ttu-id="19711-155">Отслеживание с помощью [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="19711-155">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="19711-156">Размер футболки: M</span><span class="sxs-lookup"><span data-stu-id="19711-156">T-shirt size: M</span></span>

<span data-ttu-id="19711-157">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-157">Status: Done</span></span>

<span data-ttu-id="19711-158">Включение с фильтрацией — это очень востребованная функция (около 376 голосов; второе место), которая потребует не слишком много работы и, по нашему мнению, позволит реализовать или упростить многие сценарии, в которых сейчас требуются фильтры уровня модели или более сложные запросы.</span><span class="sxs-lookup"><span data-stu-id="19711-158">Filtered Include is a highly-requested feature (~376 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="split-include"></a><span data-ttu-id="19711-159">Разделение Include</span><span class="sxs-lookup"><span data-stu-id="19711-159">Split Include</span></span>

<span data-ttu-id="19711-160">Ведущий разработчик: @smitpatel</span><span class="sxs-lookup"><span data-stu-id="19711-160">Lead developer: @smitpatel</span></span>

<span data-ttu-id="19711-161">Отслеживается по проблеме [#20892](https://github.com/dotnet/efcore/issues/20892)</span><span class="sxs-lookup"><span data-stu-id="19711-161">Tracked by [#20892](https://github.com/dotnet/efcore/issues/20892)</span></span>

<span data-ttu-id="19711-162">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-162">T-shirt size: L</span></span>

<span data-ttu-id="19711-163">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-163">Status: Done</span></span>

<span data-ttu-id="19711-164">В EF Core 3.0 изменено поведение по умолчанию для создания отдельного SQL-запроса для заданного запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="19711-164">EF Core 3.0 changed the default behavior to create a single SQL query for a given LINQ query.</span></span>
<span data-ttu-id="19711-165">Это привело к значительному ухудшению производительности запросов, использующих Include для нескольких коллекций.</span><span class="sxs-lookup"><span data-stu-id="19711-165">This caused large performance regressions for queries that use Include for multiple collections.</span></span>

<span data-ttu-id="19711-166">В EF Core 5.0 мы сохраняем новое поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="19711-166">In EF Core 5.0, we are retaining the new default behavior.</span></span>
<span data-ttu-id="19711-167">Однако теперь EF Core 5.0 разрешает создание нескольких запросов для коллекции Include, где отдельный запрос приводит к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="19711-167">However, EF Core 5.0 will now allow generation of multiple queries for collection Includes where having a single query is causing bad performance.</span></span>

## <a name="required-one-to-one-dependents"></a><span data-ttu-id="19711-168">Требуются зависимые элементы со связью "один к одному"</span><span class="sxs-lookup"><span data-stu-id="19711-168">Required one-to-one dependents</span></span>

<span data-ttu-id="19711-169">Ведущие разработчики: @AndriySvyryd и @smitpatel</span><span class="sxs-lookup"><span data-stu-id="19711-169">Lead developers: @AndriySvyryd and @smitpatel</span></span>

<span data-ttu-id="19711-170">Отслеживается по проблеме [#12100](https://github.com/dotnet/efcore/issues/12100).</span><span class="sxs-lookup"><span data-stu-id="19711-170">Tracked by [#12100](https://github.com/dotnet/efcore/issues/12100)</span></span>

<span data-ttu-id="19711-171">Размер футболки: M</span><span class="sxs-lookup"><span data-stu-id="19711-171">T-shirt size: M</span></span>

<span data-ttu-id="19711-172">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-172">Status: Done</span></span>

<span data-ttu-id="19711-173">В EF Core 3.0 все зависимые элементы, включая собственные типы, являются необязательными (например, Person.Address может иметь значение NULL).</span><span class="sxs-lookup"><span data-stu-id="19711-173">In EF Core 3.0, all dependents, including owned types are optional (e.g. Person.Address can be null).</span></span> <span data-ttu-id="19711-174">В EF Core 5.0 зависимые элементы можно настроить как обязательные.</span><span class="sxs-lookup"><span data-stu-id="19711-174">In EF Core 5.0, dependents can be configured as required.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="19711-175">Оптимизация ToTable, ToQuery, ToView, FromSql и т. п.</span><span class="sxs-lookup"><span data-stu-id="19711-175">Rationalize ToTable, ToQuery, ToView, FromSql, etc</span></span>

<span data-ttu-id="19711-176">Ведущие разработчики: @AndriySvyryd и @smitpatel</span><span class="sxs-lookup"><span data-stu-id="19711-176">Lead developers: @AndriySvyryd and @smitpatel</span></span>

<span data-ttu-id="19711-177">Отслеживание с помощью [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="19711-177">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="19711-178">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-178">T-shirt size: L</span></span>

<span data-ttu-id="19711-179">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-179">Status: Done</span></span>

<span data-ttu-id="19711-180">В предыдущих выпусках мы уже проделали определенную работу для поддержки необработанного SQL-кода, бесключевых типов и связанных с ними аспектов.</span><span class="sxs-lookup"><span data-stu-id="19711-180">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="19711-181">Однако при попытке обеспечить работу этих функций как единого целого выявляются недочеты и несоответствия.</span><span class="sxs-lookup"><span data-stu-id="19711-181">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="19711-182">Версия 5.0 должна устранить эти проблемы и обеспечить удобное определение, перенос и использование различных типов сущностей и связанных с ними запросов и артефактов баз данных.</span><span class="sxs-lookup"><span data-stu-id="19711-182">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="19711-183">Это также может потребовать внесения изменений в скомпилированный API запросов.</span><span class="sxs-lookup"><span data-stu-id="19711-183">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="19711-184">Обратите внимание, что этот пункт может привести к некоторым критическим изменениям на уровне приложения, так как некоторые функциональные возможности, которыми мы сейчас располагаем, допускают слишком вольное обращение с ними, что может быстро привести к сбою.</span><span class="sxs-lookup"><span data-stu-id="19711-184">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="19711-185">Скорее всего, мы заблокируем некоторые из этих функций и порекомендуем, чем их заменить.</span><span class="sxs-lookup"><span data-stu-id="19711-185">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="19711-186">Общие усовершенствования запросов</span><span class="sxs-lookup"><span data-stu-id="19711-186">General query enhancements</span></span>

<span data-ttu-id="19711-187">Ведущие разработчики: @smitpatel и @maumar</span><span class="sxs-lookup"><span data-stu-id="19711-187">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="19711-188">Отслеживание с помощью [вопроса, отмеченного `area-query` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="19711-188">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="19711-189">Размер футболки: XL</span><span class="sxs-lookup"><span data-stu-id="19711-189">T-shirt size: XL</span></span>

<span data-ttu-id="19711-190">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-190">Status: Done</span></span>

<span data-ttu-id="19711-191">Код преобразования запроса для EF Core 3.0 был в значительной степени переписан.</span><span class="sxs-lookup"><span data-stu-id="19711-191">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="19711-192">Благодаря этому в общем случае код запроса находится в значительно более надежном состоянии.</span><span class="sxs-lookup"><span data-stu-id="19711-192">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="19711-193">Для версии 5.0 мы не планируем вносить существенные изменения в запросы, за исключением тех, которые необходимы для поддержки метода "одна таблица на тип" и свойств навигации с пропуском.</span><span class="sxs-lookup"><span data-stu-id="19711-193">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="19711-194">Однако по-прежнему требуется значительная работа, чтобы устранить технический долг, оставшийся после масштабного изменения версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="19711-194">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="19711-195">Мы также планируем исправить многие ошибки и внести небольшие улучшения для дальнейшего улучшения общего процесса работы с запросами.</span><span class="sxs-lookup"><span data-stu-id="19711-195">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="19711-196">Процедура миграция и развертывания</span><span class="sxs-lookup"><span data-stu-id="19711-196">Migrations and deployment experience</span></span>

<span data-ttu-id="19711-197">Ведущие разработчики: @bricelam</span><span class="sxs-lookup"><span data-stu-id="19711-197">Lead developers: @bricelam</span></span>

<span data-ttu-id="19711-198">Отслеживание с помощью [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="19711-198">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="19711-199">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-199">T-shirt size: L</span></span>

<span data-ttu-id="19711-200">Состояние: Область определена/выполнено</span><span class="sxs-lookup"><span data-stu-id="19711-200">Status: Scoped/Done</span></span>

<span data-ttu-id="19711-201">Определение области: Добавление [функции пакетов миграции](https://github.com/dotnet/efcore/issues/19693) откладывается. Она появится уже после выпуска EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-201">Scoping: The [migrations bundles feature](https://github.com/dotnet/efcore/issues/19693) has been deferred until after the EF Core 5.0 release.</span></span> <span data-ttu-id="19711-202">Однако в EF Core 5.0 будет включено несколько других [улучшений, связанных с миграциями](https://github.com/dotnet/efcore/issues/19587#issuecomment-668794460).</span><span class="sxs-lookup"><span data-stu-id="19711-202">However, several other [targeted improvements related to migrations](https://github.com/dotnet/efcore/issues/19587#issuecomment-668794460) will be included in EF Core 5.0</span></span>

<span data-ttu-id="19711-203">Сейчас многие разработчики переносят свои базы данных во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="19711-203">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="19711-204">Это удобно, но не рекомендовано по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="19711-204">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="19711-205">Несколько потоков/процессов/серверов могут одновременно попытаться перенести базу данных.</span><span class="sxs-lookup"><span data-stu-id="19711-205">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="19711-206">При этом приложения могут попытаться перейти в несогласованное состояние.</span><span class="sxs-lookup"><span data-stu-id="19711-206">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="19711-207">В общем случае разрешения базы данных на изменение схемы не должны предоставляться для выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="19711-207">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="19711-208">Сложно выполнить возврат к исходному состоянию в случае сбоя.</span><span class="sxs-lookup"><span data-stu-id="19711-208">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="19711-209">Мы хотим обеспечить улучшить эту процедуру, чтобы она позволяла легко перенести базу данных во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="19711-209">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="19711-210">Предъявляются следующие требования:</span><span class="sxs-lookup"><span data-stu-id="19711-210">This should:</span></span>

* <span data-ttu-id="19711-211">Работа в Linux, Mac и Windows.</span><span class="sxs-lookup"><span data-stu-id="19711-211">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="19711-212">Хорошая совместимость с командной строкой.</span><span class="sxs-lookup"><span data-stu-id="19711-212">Be a good experience on the command line</span></span>
* <span data-ttu-id="19711-213">Поддержка сценариев с контейнерами.</span><span class="sxs-lookup"><span data-stu-id="19711-213">Support scenarios with containers</span></span>
* <span data-ttu-id="19711-214">Работа с средствами и потоками развертывания, часто применяемыми на практике.</span><span class="sxs-lookup"><span data-stu-id="19711-214">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="19711-215">Интеграция по меньшей мере с Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19711-215">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="19711-216">Результат, скорее всего, будет представлять собой множество небольших улучшений в EF Core (например, улучшенная миграция в SQLite), а также руководства и долгосрочное сотрудничество с другими группами для улучшения комплексных возможностей, выходящих за пределы EF.</span><span class="sxs-lookup"><span data-stu-id="19711-216">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="19711-217">Возможности платформ EF Core</span><span class="sxs-lookup"><span data-stu-id="19711-217">EF Core platforms experience</span></span>

<span data-ttu-id="19711-218">Ведущие разработчики: @roji и @bricelam</span><span class="sxs-lookup"><span data-stu-id="19711-218">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="19711-219">Отслеживание с помощью [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="19711-219">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="19711-220">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-220">T-shirt size: L</span></span>

<span data-ttu-id="19711-221">Состояние: Область определена/выполнено</span><span class="sxs-lookup"><span data-stu-id="19711-221">Status: Scope/Done</span></span>

<span data-ttu-id="19711-222">Определение области: Руководство по платформе и примеры публикуются для Blazor, Xamarin, WinForms и WPF.</span><span class="sxs-lookup"><span data-stu-id="19711-222">Scoping: Platform guidance and samples are published for Blazor, Xamarin, WinForms, and WPF.</span></span> <span data-ttu-id="19711-223">В настоящее время планируется работа над Xamarin и другими AOT и компоновщиками для EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="19711-223">Xamarin and other AOT/linker work is now planned for EF Core 6.0.</span></span>

<span data-ttu-id="19711-224">У нас есть хорошее руководство по использованию EF Core в традиционных веб-приложениях, работающих по принципу MVC.</span><span class="sxs-lookup"><span data-stu-id="19711-224">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="19711-225">Рекомендации для других платформ и моделей приложений отсутствуют или устарели.</span><span class="sxs-lookup"><span data-stu-id="19711-225">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="19711-226">Для EF Core 5.0 мы планируем изучить, улучшить и задокументировать процесс использования EF Core со следующими компонентами:</span><span class="sxs-lookup"><span data-stu-id="19711-226">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="19711-227">Blazor</span><span class="sxs-lookup"><span data-stu-id="19711-227">Blazor</span></span>
* <span data-ttu-id="19711-228">Xamarin, в том числе использование AOT или компоновщика</span><span class="sxs-lookup"><span data-stu-id="19711-228">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="19711-229">WinForms/WPF/WinUI и, возможно, другие платформы пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="19711-229">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="19711-230">Инфраструктуры</span><span class="sxs-lookup"><span data-stu-id="19711-230">frameworks</span></span>

<span data-ttu-id="19711-231">Результат, скорее всего, будет представлять собой множество небольших улучшений в EF Core, а также руководства и долгосрочное сотрудничество с другими группами для улучшения комплексных возможностей, выходящих за пределы EF.</span><span class="sxs-lookup"><span data-stu-id="19711-231">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="19711-232">Ниже перечислены конкретные области, которые мы планируем рассмотреть:</span><span class="sxs-lookup"><span data-stu-id="19711-232">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="19711-233">Развертывание, включая интерфейс для использования средств EF, таких как миграции</span><span class="sxs-lookup"><span data-stu-id="19711-233">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="19711-234">Модели приложений, включая Xamarin и Blazor, и, возможно, другие</span><span class="sxs-lookup"><span data-stu-id="19711-234">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="19711-235">Процедуры для SQLite, включая пространственное взаимодействие и перестроение таблиц</span><span class="sxs-lookup"><span data-stu-id="19711-235">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="19711-236">Процедуры для AOT и компоновки</span><span class="sxs-lookup"><span data-stu-id="19711-236">AOT and linking experiences</span></span>
* <span data-ttu-id="19711-237">Интеграция диагностики, включая счетчики производительности</span><span class="sxs-lookup"><span data-stu-id="19711-237">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="19711-238">Производительность</span><span class="sxs-lookup"><span data-stu-id="19711-238">Performance</span></span>

<span data-ttu-id="19711-239">Ведущий разработчик: @roji</span><span class="sxs-lookup"><span data-stu-id="19711-239">Lead developer: @roji</span></span>

<span data-ttu-id="19711-240">Отслеживание с помощью [вопроса, отмеченного `area-perf` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="19711-240">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="19711-241">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-241">T-shirt size: L</span></span>

<span data-ttu-id="19711-242">Состояние: Область определена/выполнено</span><span class="sxs-lookup"><span data-stu-id="19711-242">Status: Scoped/Done</span></span>

<span data-ttu-id="19711-243">Определение области: Существенно улучшена производительность поставщика Npgsql.</span><span class="sxs-lookup"><span data-stu-id="19711-243">Scoping: Major performance improvements in the Npgsql provider are complete.</span></span> <span data-ttu-id="19711-244">В настоящее время планируется работа над повышением производительности для EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="19711-244">Other performance work is now planned for EF Core 6.0.</span></span>

<span data-ttu-id="19711-245">Для EF Core мы планируем улучшить набор тестов производительности и внести целевые улучшения производительности для среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="19711-245">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="19711-246">Кроме того, мы планируем завершить новый API пакетной обработки ADO.NET, который находился на этапе прототипа со времени цикла выпуска версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="19711-246">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="19711-247">Кроме того, на уровне ADO.NET мы планируем дополнительно улучшить производительность для поставщика Npgsql.</span><span class="sxs-lookup"><span data-stu-id="19711-247">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="19711-248">В рамках этой задачи мы также планируем добавить счетчики производительности ADO.NET/EF Core и другие средства диагностики.</span><span class="sxs-lookup"><span data-stu-id="19711-248">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="19711-249">Документация по архитектуре и участникам</span><span class="sxs-lookup"><span data-stu-id="19711-249">Architectural/contributor documentation</span></span>

<span data-ttu-id="19711-250">Ведущий документатор: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="19711-250">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="19711-251">Отслеживание с помощью [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="19711-251">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="19711-252">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-252">T-shirt size: L</span></span>

<span data-ttu-id="19711-253">Состояние: Вырезать</span><span class="sxs-lookup"><span data-stu-id="19711-253">Status: Cut</span></span>

<span data-ttu-id="19711-254">Идея заключается в том, чтобы упростить понимание того, что происходит внутри EF Core.</span><span class="sxs-lookup"><span data-stu-id="19711-254">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="19711-255">Это может быть полезно любому пользователю EF Core, однако основная мотивация заключается в том, чтобы упростить выполнение внешними пользователями следующих операций:</span><span class="sxs-lookup"><span data-stu-id="19711-255">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="19711-256">Участие в разработке кода EF Core</span><span class="sxs-lookup"><span data-stu-id="19711-256">Contribute to the EF Core code</span></span>
* <span data-ttu-id="19711-257">Создание поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="19711-257">Create database providers</span></span>
* <span data-ttu-id="19711-258">Создание других расширений</span><span class="sxs-lookup"><span data-stu-id="19711-258">Build other extensions</span></span>

<span data-ttu-id="19711-259">Обновление: К сожалению, этот план бы слишком амбициозным.</span><span class="sxs-lookup"><span data-stu-id="19711-259">Update: Unfortunately, this plan was too ambitious.</span></span>
<span data-ttu-id="19711-260">Мы все еще считаем его важным, но, к сожалению, его не удастся реализовать в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-260">We still believe this is important, but unfortunately it won't land with EF Core 5.0.</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="19711-261">Документация по Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="19711-261">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="19711-262">Ведущий документатор: @bricelam</span><span class="sxs-lookup"><span data-stu-id="19711-262">Lead documenter: @bricelam</span></span>

<span data-ttu-id="19711-263">Отслеживание с помощью [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="19711-263">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="19711-264">Размер футболки: M</span><span class="sxs-lookup"><span data-stu-id="19711-264">T-shirt size: M</span></span>

<span data-ttu-id="19711-265">Состояние: работа завершена.</span><span class="sxs-lookup"><span data-stu-id="19711-265">Status: Completed.</span></span> <span data-ttu-id="19711-266">Новая документация [доступна на веб-сайте документации Майкрософт](/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="19711-266">The new documentation is [live on the Microsoft docs site](/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="19711-267">Группа EF также владеет поставщиком Microsoft.Data.Sqlite ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="19711-267">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="19711-268">Мы планируем полностью задокументировать этот поставщик в рамках выпуска 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-268">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="19711-269">Общая документация</span><span class="sxs-lookup"><span data-stu-id="19711-269">General documentation</span></span>

<span data-ttu-id="19711-270">Ведущий документатор: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="19711-270">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="19711-271">Отслеживание с помощью [вопросов в репозитории документации в вехе 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="19711-271">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="19711-272">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-272">T-shirt size: L</span></span>

<span data-ttu-id="19711-273">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="19711-273">Status: In-progress</span></span>

<span data-ttu-id="19711-274">Мы уже приступили к обновлению документации для выпусков 3.0 и 3.1.</span><span class="sxs-lookup"><span data-stu-id="19711-274">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="19711-275">Мы также работаем над следующими задачами:</span><span class="sxs-lookup"><span data-stu-id="19711-275">We are also working on:</span></span>

* <span data-ttu-id="19711-276">Масштабная переработка документов по началу работы, чтобы сделать их более удобными и понятными</span><span class="sxs-lookup"><span data-stu-id="19711-276">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
* <span data-ttu-id="19711-277">Реорганизация документов для облегчения поиска и добавления перекрестных ссылок</span><span class="sxs-lookup"><span data-stu-id="19711-277">Reorganization of docs to make things easier to find and to add cross-references</span></span>
* <span data-ttu-id="19711-278">Добавление дополнительных сведений и уточнений в существующие документы</span><span class="sxs-lookup"><span data-stu-id="19711-278">Adding more details and clarifications to existing docs</span></span>
* <span data-ttu-id="19711-279">Обновление существующих и добавление новых примеров</span><span class="sxs-lookup"><span data-stu-id="19711-279">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="19711-280">Исправление ошибок</span><span class="sxs-lookup"><span data-stu-id="19711-280">Fixing bugs</span></span>

<span data-ttu-id="19711-281">Отслеживание с помощью [вопроса, отмеченного `type-bug` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="19711-281">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="19711-282">Разработчики: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="19711-282">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="19711-283">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-283">T-shirt size: L</span></span>

<span data-ttu-id="19711-284">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="19711-284">Status: In-progress</span></span>

<span data-ttu-id="19711-285">На момент написания статьи у нас отобрано 135 ошибок для исправления в выпуске 5.0 (62 уже исправлены), однако они во многом пересекаются с разделом _Общие усовершенствования запросов_ выше.</span><span class="sxs-lookup"><span data-stu-id="19711-285">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="19711-286">Скорость поступления (вопросы, превратившиеся в работу в вехе) в рамках выпуска 3.0 составила около 23 вопросов в месяц.</span><span class="sxs-lookup"><span data-stu-id="19711-286">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="19711-287">Не все из них потребуется исправить в версии 5.0.</span><span class="sxs-lookup"><span data-stu-id="19711-287">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="19711-288">Ориентировочно в рамках работы над версией 5.0 мы планируем устранить 150 дополнительных проблем.</span><span class="sxs-lookup"><span data-stu-id="19711-288">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="19711-289">Небольшие усовершенствования</span><span class="sxs-lookup"><span data-stu-id="19711-289">Small enhancements</span></span>

<span data-ttu-id="19711-290">Отслеживание с помощью [вопроса, отмеченного `type-enhancement` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="19711-290">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="19711-291">Разработчики: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="19711-291">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="19711-292">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="19711-292">T-shirt size: L</span></span>

<span data-ttu-id="19711-293">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="19711-293">Status: Done</span></span>

<span data-ttu-id="19711-294">В дополнение к более масштабным функциям, описанным выше, у нас также есть множество небольших усовершенствований, запланированных для версии 5.0, которые призваны исправить мелкие недочеты.</span><span class="sxs-lookup"><span data-stu-id="19711-294">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="19711-295">Обратите внимание, что многие из этих усовершенствований также входят в более общие темы, описанные выше.</span><span class="sxs-lookup"><span data-stu-id="19711-295">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="19711-296">Выход за пределы плана</span><span class="sxs-lookup"><span data-stu-id="19711-296">Below-the-line</span></span>

<span data-ttu-id="19711-297">Отслеживание с помощью [вопроса, отмеченного `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="19711-297">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="19711-298">Это исправления ошибок и усовершенствования, которые пока **не** запланированы для выпуска 5.0, но мы постараемся включить их в зависимости от того, как будет продвигаться описанная выше работа.</span><span class="sxs-lookup"><span data-stu-id="19711-298">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="19711-299">Кроме того, при планировании мы всегда учитываем [вопросы, набравшие больше всего голосов](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="19711-299">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="19711-300">Исключение любой из подобных проблем из выпуска всегда дается нелегко, но нам нужен реалистичный план с учетом ресурсов, которыми мы располагаем.</span><span class="sxs-lookup"><span data-stu-id="19711-300">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="suggestions"></a><span data-ttu-id="19711-301">Предложения</span><span class="sxs-lookup"><span data-stu-id="19711-301">Suggestions</span></span>

<span data-ttu-id="19711-302">Нам важно ваше мнение о планировании.</span><span class="sxs-lookup"><span data-stu-id="19711-302">Your feedback on planning is important.</span></span> <span data-ttu-id="19711-303">Лучший способ указать важность проблемы — проголосовать за нее на GitHub.</span><span class="sxs-lookup"><span data-stu-id="19711-303">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="19711-304">Эти данные будут учитываться в [процессе планирования](xref:core/what-is-new/release_planning) для следующего выпуска.</span><span class="sxs-lookup"><span data-stu-id="19711-304">This data will then feed into the [planning process](xref:core/what-is-new/release_planning) for the next release.</span></span>
