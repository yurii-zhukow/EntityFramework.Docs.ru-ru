---
title: Планирование для Entity Framework Core 5.0
description: Функции, запланированные в Entity Framework Core 5.0
author: ajcvickers
ms.date: 08/22/2020
uid: core/what-is-new/ef-core-5.0/plan
ms.openlocfilehash: 5ac7a48a70959b625b7c602ee48dc08a360bee73
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618668"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="49370-103">Планирование для Entity Framework Core 5.0</span><span class="sxs-lookup"><span data-stu-id="49370-103">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="49370-104">Как было описано в [процессе планирования](xref:core/what-is-new/release-planning), мы собрали сведения от заинтересованных лиц в предварительный план для выпуска EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-104">As described in the [planning process](xref:core/what-is-new/release-planning), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49370-105">Этот план отражает все еще продолжающиеся работы.</span><span class="sxs-lookup"><span data-stu-id="49370-105">This plan is still a work-in-progress.</span></span> <span data-ttu-id="49370-106">Ничего из приведенного здесь не является обязательством.</span><span class="sxs-lookup"><span data-stu-id="49370-106">Nothing here is a commitment.</span></span> <span data-ttu-id="49370-107">Этот план является отправной точкой и будет расширяться по мере обучения.</span><span class="sxs-lookup"><span data-stu-id="49370-107">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="49370-108">Могут быть включены некоторые вещи, пока не запланированные для версии 5.0,</span><span class="sxs-lookup"><span data-stu-id="49370-108">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="49370-109">и исключены некоторые вещи, уже запланированные для этой версии.</span><span class="sxs-lookup"><span data-stu-id="49370-109">Some things currently planned for 5.0 may get punted out.</span></span>

## <a name="general-information"></a><span data-ttu-id="49370-110">Общие сведения</span><span class="sxs-lookup"><span data-stu-id="49370-110">General information</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="49370-111">Номер версии и дата выпуска</span><span class="sxs-lookup"><span data-stu-id="49370-111">Version number and release date</span></span>

<span data-ttu-id="49370-112">Сейчас выпуск EF Core 5.0 запланирован на [то же самое время, что и.NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="49370-112">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="49370-113">Версия "5.0" была выбрана для согласования с .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-113">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="49370-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="49370-114">Supported platforms</span></span>

<span data-ttu-id="49370-115">Планируется, что EF Core 5.0 будет работать на любой платформе .NET Standard 2.1, включая .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-115">EF Core 5.0 is planned to run on any .NET Standard 2.1 platform, including .NET 5.0.</span></span> <span data-ttu-id="49370-116">Это согласуется с более общей стратегией [конвергенции платформ .NET в .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="49370-116">This is part of the more general .NET wide [convergence of platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span>

<span data-ttu-id="49370-117">EF Core 5.0 не будет работать в .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="49370-117">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="49370-118">Критические изменения</span><span class="sxs-lookup"><span data-stu-id="49370-118">Breaking changes</span></span>

<span data-ttu-id="49370-119">EF Core 5.0 будет содержать некоторые [критические изменения](xref:core/what-is-new/ef-core-5.0/breaking-changes), но они будут гораздо менее серьезными, чем в случае с EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="49370-119">EF Core 5.0 will contain some [breaking changes](xref:core/what-is-new/ef-core-5.0/breaking-changes), but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="49370-120">Наша цель состоит в том, чтобы позволить подавляющему большинству приложений осуществлять обновление без нарушения работы.</span><span class="sxs-lookup"><span data-stu-id="49370-120">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="49370-121">Ожидаются некоторые критические изменения для поставщиков баз данных, особенно связанные с поддержкой метода "одна таблица на тип".</span><span class="sxs-lookup"><span data-stu-id="49370-121">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="49370-122">Однако мы ожидаем, что обновление поставщика для версии 5.0 потребует меньше работы, чем при обновлении для версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="49370-122">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="49370-123">Темы</span><span class="sxs-lookup"><span data-stu-id="49370-123">Themes</span></span>

<span data-ttu-id="49370-124">Мы определили несколько основных областей или тем, которые сформируют основу для крупномасштабных инвестиций в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-124">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="fully-transparent-many-to-many-mapping-by-convention"></a><span data-ttu-id="49370-125">Полностью прозрачное сопоставление "многие ко многим" в соответствии с соглашением</span><span class="sxs-lookup"><span data-stu-id="49370-125">Fully transparent many-to-many mapping by convention</span></span>

<span data-ttu-id="49370-126">Ведущие разработчики: @smitpatel, @AndriySvyryd и @lajones</span><span class="sxs-lookup"><span data-stu-id="49370-126">Lead developers: @smitpatel, @AndriySvyryd, and @lajones</span></span>

<span data-ttu-id="49370-127">Отслеживается по проблеме [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)</span><span class="sxs-lookup"><span data-stu-id="49370-127">Tracked by [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508)</span></span>

<span data-ttu-id="49370-128">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-128">T-shirt size: L</span></span>

<span data-ttu-id="49370-129">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-129">Status: Done</span></span>

<span data-ttu-id="49370-130">Функция "многие ко многим" является [наиболее востребованной](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (около 506 голосов) в журнале невыполненной работы GitHub.</span><span class="sxs-lookup"><span data-stu-id="49370-130">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~506 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="49370-131">Поддержку связей "многие ко многим" можно разбить на три основные области:</span><span class="sxs-lookup"><span data-stu-id="49370-131">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="49370-132">Пропуск свойств навигации — рассматривается в следующей теме.</span><span class="sxs-lookup"><span data-stu-id="49370-132">Skip navigation properties--covered by the next theme.</span></span>
* <span data-ttu-id="49370-133">Типы сущностей контейнера свойств.</span><span class="sxs-lookup"><span data-stu-id="49370-133">Property-bag entity types.</span></span> <span data-ttu-id="49370-134">Они позволяют использовать стандартный тип CLR (например, `Dictionary`) для экземпляров сущностей таким образом, чтобы для каждого типа сущности не требовался явный тип CLR.</span><span class="sxs-lookup"><span data-stu-id="49370-134">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="49370-135">Отслеживается по проблеме [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).</span><span class="sxs-lookup"><span data-stu-id="49370-135">Tracked by [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).</span></span>
* <span data-ttu-id="49370-136">Средства для простоты настройки связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="49370-136">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="49370-137">Помимо поддержки пропуска навигации, эти возможности связи "многие ко многим" перемещаются в EF Core 5.0 для обеспечения эффективной работы.</span><span class="sxs-lookup"><span data-stu-id="49370-137">In addition to the skip navigation support, we are now pulling these other areas of many-to-many into EF Core 5.0 so as to provide a complete experience.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="49370-138">Свойства навигации "многие ко многим" (или "навигации с пропуском")</span><span class="sxs-lookup"><span data-stu-id="49370-138">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="49370-139">Ведущие разработчики: @smitpatel и @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="49370-139">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="49370-140">Отслеживание с помощью [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="49370-140">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="49370-141">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-141">T-shirt size: L</span></span>

<span data-ttu-id="49370-142">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-142">Status: Done</span></span>

<span data-ttu-id="49370-143">Как описано в первой теме, поддержка "многие ко многим" имеет несколько аспектов.</span><span class="sxs-lookup"><span data-stu-id="49370-143">As described in the first theme, many-to-many support has multiple aspects.</span></span>
<span data-ttu-id="49370-144">В этой теме, в частности, отслеживается использование пропуска навигаций.</span><span class="sxs-lookup"><span data-stu-id="49370-144">This theme specifically tracks use of skip navigations.</span></span>
<span data-ttu-id="49370-145">Мы считаем, что наиболее существенным препятствием для тех, кому требуется поддержка типа "многие ко многим", заключается в невозможности использовать "естественные" связи, не ссылаясь на таблицу соединений, в бизнес-логике, такой как запросы.</span><span class="sxs-lookup"><span data-stu-id="49370-145">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span>
<span data-ttu-id="49370-146">Тип сущности таблицы соединений может по-прежнему существовать, но не должен мешать бизнес-логике.</span><span class="sxs-lookup"><span data-stu-id="49370-146">The join table entity type may still exist, but it should not get in the way of business logic.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="49370-147">Сопоставление наследования "одна таблица на тип"</span><span class="sxs-lookup"><span data-stu-id="49370-147">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="49370-148">Ведущий разработчик: @AndriySvyryd и @smitpatel</span><span class="sxs-lookup"><span data-stu-id="49370-148">Lead developer: @AndriySvyryd and @smitpatel</span></span>

<span data-ttu-id="49370-149">Отслеживание с помощью [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="49370-149">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="49370-150">Размер футболки: XL</span><span class="sxs-lookup"><span data-stu-id="49370-150">T-shirt size: XL</span></span>

<span data-ttu-id="49370-151">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-151">Status: Done</span></span>

<span data-ttu-id="49370-152">Мы работаем над методом "одна таблица на тип", так как это очень востребованная функция (около 289 голосов; третье место) и для нее требуются некоторые низкоуровневые изменения, которые, по нашему мнению, согласуются с основополагающим характером общего плана по .NET 5.</span><span class="sxs-lookup"><span data-stu-id="49370-152">We're doing TPT because it is both a highly requested feature (~289 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="49370-153">Мы предполагаем, что это приведет к критическим изменениям для поставщиков баз данных, хотя они должны быть гораздо менее серьезными, чем для версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="49370-153">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="49370-154">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="49370-154">Filtered Include</span></span>

<span data-ttu-id="49370-155">Ведущий разработчик: @maumar</span><span class="sxs-lookup"><span data-stu-id="49370-155">Lead developer: @maumar</span></span>

<span data-ttu-id="49370-156">Отслеживание с помощью [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="49370-156">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="49370-157">Размер футболки: M</span><span class="sxs-lookup"><span data-stu-id="49370-157">T-shirt size: M</span></span>

<span data-ttu-id="49370-158">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-158">Status: Done</span></span>

<span data-ttu-id="49370-159">Включение с фильтрацией — это очень востребованная функция (около 376 голосов; второе место), которая потребует не слишком много работы и, по нашему мнению, позволит реализовать или упростить многие сценарии, в которых сейчас требуются фильтры уровня модели или более сложные запросы.</span><span class="sxs-lookup"><span data-stu-id="49370-159">Filtered Include is a highly-requested feature (~376 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="split-include"></a><span data-ttu-id="49370-160">Разделение Include</span><span class="sxs-lookup"><span data-stu-id="49370-160">Split Include</span></span>

<span data-ttu-id="49370-161">Ведущий разработчик: @smitpatel</span><span class="sxs-lookup"><span data-stu-id="49370-161">Lead developer: @smitpatel</span></span>

<span data-ttu-id="49370-162">Отслеживается по проблеме [#20892](https://github.com/dotnet/efcore/issues/20892)</span><span class="sxs-lookup"><span data-stu-id="49370-162">Tracked by [#20892](https://github.com/dotnet/efcore/issues/20892)</span></span>

<span data-ttu-id="49370-163">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-163">T-shirt size: L</span></span>

<span data-ttu-id="49370-164">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-164">Status: Done</span></span>

<span data-ttu-id="49370-165">В EF Core 3.0 изменено поведение по умолчанию для создания отдельного SQL-запроса для заданного запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="49370-165">EF Core 3.0 changed the default behavior to create a single SQL query for a given LINQ query.</span></span>
<span data-ttu-id="49370-166">Это привело к значительному ухудшению производительности запросов, использующих Include для нескольких коллекций.</span><span class="sxs-lookup"><span data-stu-id="49370-166">This caused large performance regressions for queries that use Include for multiple collections.</span></span>

<span data-ttu-id="49370-167">В EF Core 5.0 мы сохраняем новое поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="49370-167">In EF Core 5.0, we are retaining the new default behavior.</span></span>
<span data-ttu-id="49370-168">Однако теперь EF Core 5.0 разрешает создание нескольких запросов для коллекции Include, где отдельный запрос приводит к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="49370-168">However, EF Core 5.0 will now allow generation of multiple queries for collection Includes where having a single query is causing bad performance.</span></span>

## <a name="required-one-to-one-dependents"></a><span data-ttu-id="49370-169">Требуются зависимые элементы со связью "один к одному"</span><span class="sxs-lookup"><span data-stu-id="49370-169">Required one-to-one dependents</span></span>

<span data-ttu-id="49370-170">Ведущие разработчики: @AndriySvyryd и @smitpatel</span><span class="sxs-lookup"><span data-stu-id="49370-170">Lead developers: @AndriySvyryd and @smitpatel</span></span>

<span data-ttu-id="49370-171">Отслеживается по проблеме [#12100](https://github.com/dotnet/efcore/issues/12100).</span><span class="sxs-lookup"><span data-stu-id="49370-171">Tracked by [#12100](https://github.com/dotnet/efcore/issues/12100)</span></span>

<span data-ttu-id="49370-172">Размер футболки: M</span><span class="sxs-lookup"><span data-stu-id="49370-172">T-shirt size: M</span></span>

<span data-ttu-id="49370-173">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-173">Status: Done</span></span>

<span data-ttu-id="49370-174">В EF Core 3.0 все зависимые элементы, включая собственные типы, являются необязательными (например, Person.Address может иметь значение NULL).</span><span class="sxs-lookup"><span data-stu-id="49370-174">In EF Core 3.0, all dependents, including owned types are optional (e.g. Person.Address can be null).</span></span> <span data-ttu-id="49370-175">В EF Core 5.0 зависимые элементы можно настроить как обязательные.</span><span class="sxs-lookup"><span data-stu-id="49370-175">In EF Core 5.0, dependents can be configured as required.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="49370-176">Оптимизация ToTable, ToQuery, ToView, FromSql и т. п.</span><span class="sxs-lookup"><span data-stu-id="49370-176">Rationalize ToTable, ToQuery, ToView, FromSql, etc</span></span>

<span data-ttu-id="49370-177">Ведущие разработчики: @AndriySvyryd и @smitpatel</span><span class="sxs-lookup"><span data-stu-id="49370-177">Lead developers: @AndriySvyryd and @smitpatel</span></span>

<span data-ttu-id="49370-178">Отслеживание с помощью [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="49370-178">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="49370-179">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-179">T-shirt size: L</span></span>

<span data-ttu-id="49370-180">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-180">Status: Done</span></span>

<span data-ttu-id="49370-181">В предыдущих выпусках мы уже проделали определенную работу для поддержки необработанного SQL-кода, бесключевых типов и связанных с ними аспектов.</span><span class="sxs-lookup"><span data-stu-id="49370-181">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="49370-182">Однако при попытке обеспечить работу этих функций как единого целого выявляются недочеты и несоответствия.</span><span class="sxs-lookup"><span data-stu-id="49370-182">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="49370-183">Версия 5.0 должна устранить эти проблемы и обеспечить удобное определение, перенос и использование различных типов сущностей и связанных с ними запросов и артефактов баз данных.</span><span class="sxs-lookup"><span data-stu-id="49370-183">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="49370-184">Это также может потребовать внесения изменений в скомпилированный API запросов.</span><span class="sxs-lookup"><span data-stu-id="49370-184">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="49370-185">Обратите внимание, что этот пункт может привести к некоторым критическим изменениям на уровне приложения, так как некоторые функциональные возможности, которыми мы сейчас располагаем, допускают слишком вольное обращение с ними, что может быстро привести к сбою.</span><span class="sxs-lookup"><span data-stu-id="49370-185">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="49370-186">Скорее всего, мы заблокируем некоторые из этих функций и порекомендуем, чем их заменить.</span><span class="sxs-lookup"><span data-stu-id="49370-186">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="49370-187">Общие усовершенствования запросов</span><span class="sxs-lookup"><span data-stu-id="49370-187">General query enhancements</span></span>

<span data-ttu-id="49370-188">Ведущие разработчики: @smitpatel и @maumar</span><span class="sxs-lookup"><span data-stu-id="49370-188">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="49370-189">Отслеживание с помощью [вопроса, отмеченного `area-query` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="49370-189">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="49370-190">Размер футболки: XL</span><span class="sxs-lookup"><span data-stu-id="49370-190">T-shirt size: XL</span></span>

<span data-ttu-id="49370-191">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-191">Status: Done</span></span>

<span data-ttu-id="49370-192">Код преобразования запроса для EF Core 3.0 был в значительной степени переписан.</span><span class="sxs-lookup"><span data-stu-id="49370-192">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="49370-193">Благодаря этому в общем случае код запроса находится в значительно более надежном состоянии.</span><span class="sxs-lookup"><span data-stu-id="49370-193">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="49370-194">Для версии 5.0 мы не планируем вносить существенные изменения в запросы, за исключением тех, которые необходимы для поддержки метода "одна таблица на тип" и свойств навигации с пропуском.</span><span class="sxs-lookup"><span data-stu-id="49370-194">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="49370-195">Однако по-прежнему требуется значительная работа, чтобы устранить технический долг, оставшийся после масштабного изменения версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="49370-195">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="49370-196">Мы также планируем исправить многие ошибки и внести небольшие улучшения для дальнейшего улучшения общего процесса работы с запросами.</span><span class="sxs-lookup"><span data-stu-id="49370-196">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="49370-197">Процедура миграция и развертывания</span><span class="sxs-lookup"><span data-stu-id="49370-197">Migrations and deployment experience</span></span>

<span data-ttu-id="49370-198">Ведущие разработчики: @bricelam</span><span class="sxs-lookup"><span data-stu-id="49370-198">Lead developers: @bricelam</span></span>

<span data-ttu-id="49370-199">Отслеживание с помощью [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="49370-199">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="49370-200">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-200">T-shirt size: L</span></span>

<span data-ttu-id="49370-201">Состояние: Область определена/выполнено</span><span class="sxs-lookup"><span data-stu-id="49370-201">Status: Scoped/Done</span></span>

<span data-ttu-id="49370-202">Определение области: Добавление [функции пакетов миграции](https://github.com/dotnet/efcore/issues/19693) откладывается. Она появится уже после выпуска EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-202">Scoping: The [migrations bundles feature](https://github.com/dotnet/efcore/issues/19693) has been deferred until after the EF Core 5.0 release.</span></span> <span data-ttu-id="49370-203">Однако в EF Core 5.0 будет включено несколько других [улучшений, связанных с миграциями](https://github.com/dotnet/efcore/issues/19587#issuecomment-668794460).</span><span class="sxs-lookup"><span data-stu-id="49370-203">However, several other [targeted improvements related to migrations](https://github.com/dotnet/efcore/issues/19587#issuecomment-668794460) will be included in EF Core 5.0</span></span>

<span data-ttu-id="49370-204">Сейчас многие разработчики переносят свои базы данных во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="49370-204">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="49370-205">Это удобно, но не рекомендовано по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="49370-205">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="49370-206">Несколько потоков/процессов/серверов могут одновременно попытаться перенести базу данных.</span><span class="sxs-lookup"><span data-stu-id="49370-206">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="49370-207">При этом приложения могут попытаться перейти в несогласованное состояние.</span><span class="sxs-lookup"><span data-stu-id="49370-207">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="49370-208">В общем случае разрешения базы данных на изменение схемы не должны предоставляться для выполнения приложения.</span><span class="sxs-lookup"><span data-stu-id="49370-208">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="49370-209">Сложно выполнить возврат к исходному состоянию в случае сбоя.</span><span class="sxs-lookup"><span data-stu-id="49370-209">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="49370-210">Мы хотим обеспечить улучшить эту процедуру, чтобы она позволяла легко перенести базу данных во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="49370-210">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="49370-211">Предъявляются следующие требования:</span><span class="sxs-lookup"><span data-stu-id="49370-211">This should:</span></span>

* <span data-ttu-id="49370-212">Работа в Linux, Mac и Windows.</span><span class="sxs-lookup"><span data-stu-id="49370-212">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="49370-213">Хорошая совместимость с командной строкой.</span><span class="sxs-lookup"><span data-stu-id="49370-213">Be a good experience on the command line</span></span>
* <span data-ttu-id="49370-214">Поддержка сценариев с контейнерами.</span><span class="sxs-lookup"><span data-stu-id="49370-214">Support scenarios with containers</span></span>
* <span data-ttu-id="49370-215">Работа с средствами и потоками развертывания, часто применяемыми на практике.</span><span class="sxs-lookup"><span data-stu-id="49370-215">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="49370-216">Интеграция по меньшей мере с Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49370-216">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="49370-217">Результат, скорее всего, будет представлять собой множество небольших улучшений в EF Core (например, улучшенная миграция в SQLite), а также руководства и долгосрочное сотрудничество с другими группами для улучшения комплексных возможностей, выходящих за пределы EF.</span><span class="sxs-lookup"><span data-stu-id="49370-217">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="49370-218">Возможности платформ EF Core</span><span class="sxs-lookup"><span data-stu-id="49370-218">EF Core platforms experience</span></span>

<span data-ttu-id="49370-219">Ведущие разработчики: @roji и @bricelam</span><span class="sxs-lookup"><span data-stu-id="49370-219">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="49370-220">Отслеживание с помощью [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="49370-220">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="49370-221">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-221">T-shirt size: L</span></span>

<span data-ttu-id="49370-222">Состояние: Область определена/выполнено</span><span class="sxs-lookup"><span data-stu-id="49370-222">Status: Scope/Done</span></span>

<span data-ttu-id="49370-223">Определение области: Руководство по платформе и примеры публикуются для Blazor, Xamarin, WinForms и WPF.</span><span class="sxs-lookup"><span data-stu-id="49370-223">Scoping: Platform guidance and samples are published for Blazor, Xamarin, WinForms, and WPF.</span></span> <span data-ttu-id="49370-224">В настоящее время планируется работа над Xamarin и другими AOT и компоновщиками для EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="49370-224">Xamarin and other AOT/linker work is now planned for EF Core 6.0.</span></span>

<span data-ttu-id="49370-225">У нас есть хорошее руководство по использованию EF Core в традиционных веб-приложениях, работающих по принципу MVC.</span><span class="sxs-lookup"><span data-stu-id="49370-225">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="49370-226">Рекомендации для других платформ и моделей приложений отсутствуют или устарели.</span><span class="sxs-lookup"><span data-stu-id="49370-226">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="49370-227">Для EF Core 5.0 мы планируем изучить, улучшить и задокументировать процесс использования EF Core со следующими компонентами:</span><span class="sxs-lookup"><span data-stu-id="49370-227">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="49370-228">Blazor</span><span class="sxs-lookup"><span data-stu-id="49370-228">Blazor</span></span>
* <span data-ttu-id="49370-229">Xamarin, в том числе использование AOT или компоновщика</span><span class="sxs-lookup"><span data-stu-id="49370-229">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="49370-230">WinForms/WPF/WinUI и, возможно, другие платформы пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="49370-230">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="49370-231">Инфраструктуры</span><span class="sxs-lookup"><span data-stu-id="49370-231">frameworks</span></span>

<span data-ttu-id="49370-232">Результат, скорее всего, будет представлять собой множество небольших улучшений в EF Core, а также руководства и долгосрочное сотрудничество с другими группами для улучшения комплексных возможностей, выходящих за пределы EF.</span><span class="sxs-lookup"><span data-stu-id="49370-232">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="49370-233">Ниже перечислены конкретные области, которые мы планируем рассмотреть:</span><span class="sxs-lookup"><span data-stu-id="49370-233">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="49370-234">Развертывание, включая интерфейс для использования средств EF, таких как миграции</span><span class="sxs-lookup"><span data-stu-id="49370-234">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="49370-235">Модели приложений, включая Xamarin и Blazor, и, возможно, другие</span><span class="sxs-lookup"><span data-stu-id="49370-235">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="49370-236">Процедуры для SQLite, включая пространственное взаимодействие и перестроение таблиц</span><span class="sxs-lookup"><span data-stu-id="49370-236">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="49370-237">Процедуры для AOT и компоновки</span><span class="sxs-lookup"><span data-stu-id="49370-237">AOT and linking experiences</span></span>
* <span data-ttu-id="49370-238">Интеграция диагностики, включая счетчики производительности</span><span class="sxs-lookup"><span data-stu-id="49370-238">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="49370-239">Производительность</span><span class="sxs-lookup"><span data-stu-id="49370-239">Performance</span></span>

<span data-ttu-id="49370-240">Ведущий разработчик: @roji</span><span class="sxs-lookup"><span data-stu-id="49370-240">Lead developer: @roji</span></span>

<span data-ttu-id="49370-241">Отслеживание с помощью [вопроса, отмеченного `area-perf` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="49370-241">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="49370-242">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-242">T-shirt size: L</span></span>

<span data-ttu-id="49370-243">Состояние: Область определена/выполнено</span><span class="sxs-lookup"><span data-stu-id="49370-243">Status: Scoped/Done</span></span>

<span data-ttu-id="49370-244">Определение области: Существенно улучшена производительность поставщика Npgsql.</span><span class="sxs-lookup"><span data-stu-id="49370-244">Scoping: Major performance improvements in the Npgsql provider are complete.</span></span> <span data-ttu-id="49370-245">В настоящее время планируется работа над повышением производительности для EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="49370-245">Other performance work is now planned for EF Core 6.0.</span></span>

<span data-ttu-id="49370-246">Для EF Core мы планируем улучшить набор тестов производительности и внести целевые улучшения производительности для среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="49370-246">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="49370-247">Кроме того, мы планируем завершить новый API пакетной обработки ADO.NET, который находился на этапе прототипа со времени цикла выпуска версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="49370-247">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="49370-248">Кроме того, на уровне ADO.NET мы планируем дополнительно улучшить производительность для поставщика Npgsql.</span><span class="sxs-lookup"><span data-stu-id="49370-248">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="49370-249">В рамках этой задачи мы также планируем добавить счетчики производительности ADO.NET/EF Core и другие средства диагностики.</span><span class="sxs-lookup"><span data-stu-id="49370-249">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="49370-250">Документация по архитектуре и участникам</span><span class="sxs-lookup"><span data-stu-id="49370-250">Architectural/contributor documentation</span></span>

<span data-ttu-id="49370-251">Ведущий документатор: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="49370-251">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="49370-252">Отслеживание с помощью [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="49370-252">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="49370-253">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-253">T-shirt size: L</span></span>

<span data-ttu-id="49370-254">Состояние: Вырезать</span><span class="sxs-lookup"><span data-stu-id="49370-254">Status: Cut</span></span>

<span data-ttu-id="49370-255">Идея заключается в том, чтобы упростить понимание того, что происходит внутри EF Core.</span><span class="sxs-lookup"><span data-stu-id="49370-255">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="49370-256">Это может быть полезно любому пользователю EF Core, однако основная мотивация заключается в том, чтобы упростить выполнение внешними пользователями следующих операций:</span><span class="sxs-lookup"><span data-stu-id="49370-256">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="49370-257">Участие в разработке кода EF Core</span><span class="sxs-lookup"><span data-stu-id="49370-257">Contribute to the EF Core code</span></span>
* <span data-ttu-id="49370-258">Создание поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="49370-258">Create database providers</span></span>
* <span data-ttu-id="49370-259">Создание других расширений</span><span class="sxs-lookup"><span data-stu-id="49370-259">Build other extensions</span></span>

<span data-ttu-id="49370-260">Обновление: К сожалению, этот план бы слишком амбициозным.</span><span class="sxs-lookup"><span data-stu-id="49370-260">Update: Unfortunately, this plan was too ambitious.</span></span>
<span data-ttu-id="49370-261">Мы все еще считаем его важным, но, к сожалению, его не удастся реализовать в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-261">We still believe this is important, but unfortunately it won't land with EF Core 5.0.</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="49370-262">Документация по Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="49370-262">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="49370-263">Ведущий документатор: @bricelam</span><span class="sxs-lookup"><span data-stu-id="49370-263">Lead documenter: @bricelam</span></span>

<span data-ttu-id="49370-264">Отслеживание с помощью [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="49370-264">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="49370-265">Размер футболки: M</span><span class="sxs-lookup"><span data-stu-id="49370-265">T-shirt size: M</span></span>

<span data-ttu-id="49370-266">Состояние: работа завершена.</span><span class="sxs-lookup"><span data-stu-id="49370-266">Status: Completed.</span></span> <span data-ttu-id="49370-267">Новая документация [доступна на веб-сайте документации Майкрософт](/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="49370-267">The new documentation is [live on the Microsoft docs site](/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="49370-268">Группа EF также владеет поставщиком Microsoft.Data.Sqlite ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="49370-268">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="49370-269">Мы планируем полностью задокументировать этот поставщик в рамках выпуска 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-269">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="49370-270">Общая документация</span><span class="sxs-lookup"><span data-stu-id="49370-270">General documentation</span></span>

<span data-ttu-id="49370-271">Ведущий документатор: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="49370-271">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="49370-272">Отслеживание с помощью [вопросов в репозитории документации в вехе 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="49370-272">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="49370-273">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-273">T-shirt size: L</span></span>

<span data-ttu-id="49370-274">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="49370-274">Status: In-progress</span></span>

<span data-ttu-id="49370-275">Мы уже приступили к обновлению документации для выпусков 3.0 и 3.1.</span><span class="sxs-lookup"><span data-stu-id="49370-275">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="49370-276">Мы также работаем над следующими задачами:</span><span class="sxs-lookup"><span data-stu-id="49370-276">We are also working on:</span></span>

* <span data-ttu-id="49370-277">Масштабная переработка документов по началу работы, чтобы сделать их более удобными и понятными</span><span class="sxs-lookup"><span data-stu-id="49370-277">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
* <span data-ttu-id="49370-278">Реорганизация документов для облегчения поиска и добавления перекрестных ссылок</span><span class="sxs-lookup"><span data-stu-id="49370-278">Reorganization of docs to make things easier to find and to add cross-references</span></span>
* <span data-ttu-id="49370-279">Добавление дополнительных сведений и уточнений в существующие документы</span><span class="sxs-lookup"><span data-stu-id="49370-279">Adding more details and clarifications to existing docs</span></span>
* <span data-ttu-id="49370-280">Обновление существующих и добавление новых примеров</span><span class="sxs-lookup"><span data-stu-id="49370-280">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="49370-281">Исправление ошибок</span><span class="sxs-lookup"><span data-stu-id="49370-281">Fixing bugs</span></span>

<span data-ttu-id="49370-282">Отслеживание с помощью [вопроса, отмеченного `type-bug` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="49370-282">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="49370-283">Разработчики: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="49370-283">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="49370-284">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-284">T-shirt size: L</span></span>

<span data-ttu-id="49370-285">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="49370-285">Status: In-progress</span></span>

<span data-ttu-id="49370-286">На момент написания статьи у нас отобрано 135 ошибок для исправления в выпуске 5.0 (62 уже исправлены), однако они во многом пересекаются с разделом _Общие усовершенствования запросов_ выше.</span><span class="sxs-lookup"><span data-stu-id="49370-286">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="49370-287">Скорость поступления (вопросы, превратившиеся в работу в вехе) в рамках выпуска 3.0 составила около 23 вопросов в месяц.</span><span class="sxs-lookup"><span data-stu-id="49370-287">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="49370-288">Не все из них потребуется исправить в версии 5.0.</span><span class="sxs-lookup"><span data-stu-id="49370-288">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="49370-289">Ориентировочно в рамках работы над версией 5.0 мы планируем устранить 150 дополнительных проблем.</span><span class="sxs-lookup"><span data-stu-id="49370-289">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="49370-290">Небольшие усовершенствования</span><span class="sxs-lookup"><span data-stu-id="49370-290">Small enhancements</span></span>

<span data-ttu-id="49370-291">Отслеживание с помощью [вопроса, отмеченного `type-enhancement` в вехе 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="49370-291">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="49370-292">Разработчики: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="49370-292">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="49370-293">Размер футболки: L</span><span class="sxs-lookup"><span data-stu-id="49370-293">T-shirt size: L</span></span>

<span data-ttu-id="49370-294">Состояние: Готово</span><span class="sxs-lookup"><span data-stu-id="49370-294">Status: Done</span></span>

<span data-ttu-id="49370-295">В дополнение к более масштабным функциям, описанным выше, у нас также есть множество небольших усовершенствований, запланированных для версии 5.0, которые призваны исправить мелкие недочеты.</span><span class="sxs-lookup"><span data-stu-id="49370-295">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="49370-296">Обратите внимание, что многие из этих усовершенствований также входят в более общие темы, описанные выше.</span><span class="sxs-lookup"><span data-stu-id="49370-296">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="49370-297">Выход за пределы плана</span><span class="sxs-lookup"><span data-stu-id="49370-297">Below-the-line</span></span>

<span data-ttu-id="49370-298">Отслеживание с помощью [вопроса, отмеченного `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="49370-298">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="49370-299">Это исправления ошибок и усовершенствования, которые пока **не** запланированы для выпуска 5.0, но мы постараемся включить их в зависимости от того, как будет продвигаться описанная выше работа.</span><span class="sxs-lookup"><span data-stu-id="49370-299">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="49370-300">Кроме того, при планировании мы всегда учитываем [вопросы, набравшие больше всего голосов](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="49370-300">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="49370-301">Исключение любой из подобных проблем из выпуска всегда дается нелегко, но нам нужен реалистичный план с учетом ресурсов, которыми мы располагаем.</span><span class="sxs-lookup"><span data-stu-id="49370-301">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="suggestions"></a><span data-ttu-id="49370-302">Предложения</span><span class="sxs-lookup"><span data-stu-id="49370-302">Suggestions</span></span>

<span data-ttu-id="49370-303">Нам важно ваше мнение о планировании.</span><span class="sxs-lookup"><span data-stu-id="49370-303">Your feedback on planning is important.</span></span> <span data-ttu-id="49370-304">Лучший способ указать важность проблемы — проголосовать за нее на GitHub.</span><span class="sxs-lookup"><span data-stu-id="49370-304">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="49370-305">Эти данные будут учитываться в [процессе планирования](xref:core/what-is-new/release-planning) для следующего выпуска.</span><span class="sxs-lookup"><span data-stu-id="49370-305">This data will then feed into the [planning process](xref:core/what-is-new/release-planning) for the next release.</span></span>
