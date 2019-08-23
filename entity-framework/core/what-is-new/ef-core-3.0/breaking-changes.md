---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565305"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1f0a5-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="1f0a5-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f0a5-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1f0a5-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1f0a5-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1f0a5-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="1f0a5-107">Сводка</span><span class="sxs-lookup"><span data-stu-id="1f0a5-107">Summary</span></span>

| <span data-ttu-id="1f0a5-108">**Критические изменения**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="1f0a5-109">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="1f0a5-110">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="1f0a5-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="1f0a5-111">High</span><span class="sxs-lookup"><span data-stu-id="1f0a5-111">High</span></span>       |
| [<span data-ttu-id="1f0a5-112">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="1f0a5-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="1f0a5-113">High</span><span class="sxs-lookup"><span data-stu-id="1f0a5-113">High</span></span>      |
| [<span data-ttu-id="1f0a5-114">Программа командной строки EF Core "dotnet ef" больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="1f0a5-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="1f0a5-115">High</span><span class="sxs-lookup"><span data-stu-id="1f0a5-115">High</span></span>      |
| [<span data-ttu-id="1f0a5-116">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="1f0a5-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="1f0a5-117">High</span><span class="sxs-lookup"><span data-stu-id="1f0a5-117">High</span></span>      |
| [<span data-ttu-id="1f0a5-118">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="1f0a5-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="1f0a5-119">High</span><span class="sxs-lookup"><span data-stu-id="1f0a5-119">High</span></span>      |
| [<span data-ttu-id="1f0a5-120">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f0a5-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="1f0a5-121">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-121">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-122">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="1f0a5-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="1f0a5-123">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-123">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-124">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="1f0a5-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="1f0a5-125">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-125">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-126">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="1f0a5-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="1f0a5-127">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-127">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-128">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="1f0a5-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="1f0a5-129">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-129">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-130">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="1f0a5-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="1f0a5-131">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-131">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-132">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="1f0a5-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="1f0a5-133">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-133">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-134">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="1f0a5-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="1f0a5-135">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-135">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-136">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="1f0a5-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="1f0a5-137">Средняя</span><span class="sxs-lookup"><span data-stu-id="1f0a5-137">Medium</span></span>      |
| [<span data-ttu-id="1f0a5-138">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="1f0a5-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="1f0a5-139">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-139">Low</span></span>      |
| [<span data-ttu-id="1f0a5-140">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="1f0a5-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="1f0a5-141">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-141">Low</span></span>      |
| [<span data-ttu-id="1f0a5-142">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="1f0a5-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="1f0a5-143">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-143">Low</span></span>      |
| [<span data-ttu-id="1f0a5-144">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="1f0a5-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="1f0a5-145">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-145">Low</span></span>      |
| [<span data-ttu-id="1f0a5-146">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="1f0a5-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="1f0a5-147">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-147">Low</span></span>      |
| [<span data-ttu-id="1f0a5-148">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="1f0a5-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="1f0a5-149">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-149">Low</span></span>      |
| [<span data-ttu-id="1f0a5-150">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="1f0a5-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="1f0a5-151">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-151">Low</span></span>      |
| [<span data-ttu-id="1f0a5-152">Соглашение для свойства внешнего ключа больше не сопоставляет то же имя, что у свойства субъекта</span><span class="sxs-lookup"><span data-stu-id="1f0a5-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="1f0a5-153">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-153">Low</span></span>      |
| [<span data-ttu-id="1f0a5-154">Подключение к базе данных теперь закрывается, если оно больше не используется, до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="1f0a5-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="1f0a5-155">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-155">Low</span></span>      |
| [<span data-ttu-id="1f0a5-156">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="1f0a5-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="1f0a5-157">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-157">Low</span></span>      |
| [<span data-ttu-id="1f0a5-158">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="1f0a5-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="1f0a5-159">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-159">Low</span></span>      |
| [<span data-ttu-id="1f0a5-160">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="1f0a5-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="1f0a5-161">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-161">Low</span></span>      |
| [<span data-ttu-id="1f0a5-162">AddDbContext/AddDbContextPool больше не вызывает методы AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1f0a5-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="1f0a5-163">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-163">Low</span></span>      |
| [<span data-ttu-id="1f0a5-164">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="1f0a5-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="1f0a5-165">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-165">Low</span></span>      |
| [<span data-ttu-id="1f0a5-166">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1f0a5-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="1f0a5-167">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-167">Low</span></span>      |
| [<span data-ttu-id="1f0a5-168">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="1f0a5-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="1f0a5-169">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-169">Low</span></span>      |
| [<span data-ttu-id="1f0a5-170">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="1f0a5-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="1f0a5-171">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-171">Low</span></span>      |
| [<span data-ttu-id="1f0a5-172">Создание слишком многих внутренних поставщиков служб теперь по умолчанию является ошибкой</span><span class="sxs-lookup"><span data-stu-id="1f0a5-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="1f0a5-173">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-173">Low</span></span>      |
| [<span data-ttu-id="1f0a5-174">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="1f0a5-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="1f0a5-175">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-175">Low</span></span>      |
| [<span data-ttu-id="1f0a5-176">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="1f0a5-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="1f0a5-177">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-177">Low</span></span>      |
| [<span data-ttu-id="1f0a5-178">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1f0a5-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="1f0a5-179">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-179">Low</span></span>      |
| [<span data-ttu-id="1f0a5-180">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="1f0a5-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="1f0a5-181">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-181">Low</span></span>      |
| [<span data-ttu-id="1f0a5-182">EF Core больше не отправляет PRAGMA для принудительного применения внешних ключей SQLite</span><span class="sxs-lookup"><span data-stu-id="1f0a5-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="1f0a5-183">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-183">Low</span></span>      |
| [<span data-ttu-id="1f0a5-184">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1f0a5-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="1f0a5-185">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-185">Low</span></span>      |
| [<span data-ttu-id="1f0a5-186">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="1f0a5-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="1f0a5-187">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-187">Low</span></span>      |
| [<span data-ttu-id="1f0a5-188">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="1f0a5-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="1f0a5-189">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-189">Low</span></span>      |
| [<span data-ttu-id="1f0a5-190">Идентификаторы миграции теперь создаются с использованием календаря инвариантных языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="1f0a5-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="1f0a5-191">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-191">Low</span></span>      |
| [<span data-ttu-id="1f0a5-192">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="1f0a5-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="1f0a5-193">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-193">Low</span></span>      |
| [<span data-ttu-id="1f0a5-194">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="1f0a5-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="1f0a5-195">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-195">Low</span></span>      |
| [<span data-ttu-id="1f0a5-196">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="1f0a5-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="1f0a5-197">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-197">Low</span></span>      |
| [<span data-ttu-id="1f0a5-198">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="1f0a5-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="1f0a5-199">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-199">Low</span></span>      |
| [<span data-ttu-id="1f0a5-200">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="1f0a5-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="1f0a5-201">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-201">Low</span></span>      |
| [<span data-ttu-id="1f0a5-202">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1f0a5-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="1f0a5-203">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-203">Low</span></span>      |
| [<span data-ttu-id="1f0a5-204">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1f0a5-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="1f0a5-205">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-205">Low</span></span>      |
| [<span data-ttu-id="1f0a5-206">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="1f0a5-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="1f0a5-207">Low</span><span class="sxs-lookup"><span data-stu-id="1f0a5-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1f0a5-208">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="1f0a5-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1f0a5-209">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1f0a5-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1f0a5-210">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-211">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-211">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-212">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1f0a5-213">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1f0a5-214">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-214">**New behavior**</span></span>

<span data-ttu-id="1f0a5-215">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1f0a5-216">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1f0a5-217">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-217">**Why**</span></span>

<span data-ttu-id="1f0a5-218">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1f0a5-219">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1f0a5-220">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1f0a5-221">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1f0a5-222">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1f0a5-223">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1f0a5-224">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-224">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-225">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="1f0a5-226">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="1f0a5-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="1f0a5-227">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="1f0a5-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="1f0a5-228">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1f0a5-229">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-229">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-230">До версии 3.0 EF Core был предназначен для .NET Standard 2.0 и выполнялся на всех платформах, поддерживающих этот стандарт, включая .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="1f0a5-231">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-231">**New behavior**</span></span>

<span data-ttu-id="1f0a5-232">Начиная с версии 3.0, EF Core предназначен для .NET Standard 2.1 и выполняется на всех платформах, поддерживающих этот стандарт.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="1f0a5-233">Сюда не входит .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="1f0a5-234">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-234">**Why**</span></span>

<span data-ttu-id="1f0a5-235">Это часть стратегического решения по технологиям .NET, направленного на работу над .NET Core и другими современными платформами .NET, такими как Xamarin.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="1f0a5-236">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-236">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-237">Рассмотрите возможность перехода на современные платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="1f0a5-238">Если это невозможно, продолжайте использовать EF Core 2.1 или EF Core 2.2, которые поддерживают .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1f0a5-239">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f0a5-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1f0a5-240">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="1f0a5-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1f0a5-241">Это изменение реализовано в ASP.NET Core 3.0, предварительная версия 1.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1f0a5-242">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-242">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-243">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1f0a5-244">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-244">**New behavior**</span></span>

<span data-ttu-id="1f0a5-245">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1f0a5-246">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-246">**Why**</span></span>

<span data-ttu-id="1f0a5-247">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1f0a5-248">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1f0a5-249">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1f0a5-250">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1f0a5-251">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-251">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-252">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="1f0a5-253">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="1f0a5-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="1f0a5-254">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="1f0a5-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="1f0a5-255">Это изменение реализовано в EF Core 3.0, предварительная версия 4, и соответствующей версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="1f0a5-256">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-256">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-257">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="1f0a5-258">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-258">**New behavior**</span></span>

<span data-ttu-id="1f0a5-259">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="1f0a5-260">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-260">**Why**</span></span>

<span data-ttu-id="1f0a5-261">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="1f0a5-262">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-262">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-263">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="1f0a5-264">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1f0a5-265">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="1f0a5-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1f0a5-266">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="1f0a5-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1f0a5-267">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-268">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-268">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-269">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1f0a5-270">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-270">**New behavior**</span></span>

<span data-ttu-id="1f0a5-271">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1f0a5-272">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1f0a5-273">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1f0a5-274">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1f0a5-275">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1f0a5-276">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-276">**Why**</span></span>

<span data-ttu-id="1f0a5-277">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1f0a5-278">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1f0a5-279">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-279">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-280">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="1f0a5-281">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="1f0a5-282">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="1f0a5-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="1f0a5-283">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1f0a5-284">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-284">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-285">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="1f0a5-286">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-286">**New behavior**</span></span>

<span data-ttu-id="1f0a5-287">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="1f0a5-288">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="1f0a5-289">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-289">**Why**</span></span>

<span data-ttu-id="1f0a5-290">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="1f0a5-291">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-291">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-292">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="1f0a5-293">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="1f0a5-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="1f0a5-294">Отслеживание вопроса № 13518</span><span class="sxs-lookup"><span data-stu-id="1f0a5-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="1f0a5-295">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1f0a5-296">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-296">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-297">До EF Core 3.0 один и тот же экземпляр сущности будет использоваться для каждого вхождения сущности с заданным типом и ИД.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="1f0a5-298">Это соответствует поведению запросов отслеживания.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="1f0a5-299">Например, запрос</span><span class="sxs-lookup"><span data-stu-id="1f0a5-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="1f0a5-300">возвращает тот же экземпляр `Category` для каждого `Product`, связанного с заданной категорией.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="1f0a5-301">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-301">**New behavior**</span></span>

<span data-ttu-id="1f0a5-302">Начиная с EF Core 3.0, при обнаружении сущности с заданным типом и ИД в разных местах возвращенного графа будут создаваться различные экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="1f0a5-303">Например, вышеприведенный запрос теперь будет возвращать новый экземпляр `Category` для каждого `Product`, даже если два продукта связаны с одной категорией.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="1f0a5-304">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-304">**Why**</span></span>

<span data-ttu-id="1f0a5-305">Разрешение идентификаторов (то есть определение того, что сущность имеет тот же тип и идентификатор, что и обнаруженная ранее сущность) добавляет дополнительную нагрузку на производительность и память.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="1f0a5-306">Обычно это идет вразрез с причинами использования запросов без отслеживания.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="1f0a5-307">Кроме того, хотя разрешение идентификаторов иногда может быть полезным, оно не требуется, если сущности должны быть сериализованы и отправлены клиенту, что является распространенным случаем для неотслеживаемых запросов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="1f0a5-308">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-308">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-309">Используйте запрос с отслеживанием, если требуется разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="1f0a5-310">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="1f0a5-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="1f0a5-311">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="1f0a5-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1f0a5-312">Это изменение отменено в выпуске EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1f0a5-313">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="1f0a5-314">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1f0a5-315">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="1f0a5-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1f0a5-316">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="1f0a5-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1f0a5-317">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1f0a5-318">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-318">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-319">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1f0a5-320">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1f0a5-321">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-321">**New behavior**</span></span>

<span data-ttu-id="1f0a5-322">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1f0a5-323">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-323">**Why**</span></span>

<span data-ttu-id="1f0a5-324">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1f0a5-325">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-325">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-326">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1f0a5-327">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-327">This can be avoided by:</span></span>
* <span data-ttu-id="1f0a5-328">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="1f0a5-329">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1f0a5-330">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1f0a5-331">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1f0a5-332">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="1f0a5-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1f0a5-333">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="1f0a5-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1f0a5-334">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-335">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-335">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-336">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1f0a5-337">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-337">**New behavior**</span></span>

<span data-ttu-id="1f0a5-338">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1f0a5-339">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1f0a5-340">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1f0a5-341">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-341">**Why**</span></span>

<span data-ttu-id="1f0a5-342">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1f0a5-343">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-343">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-344">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1f0a5-345">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1f0a5-346">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1f0a5-347">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1f0a5-348">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="1f0a5-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1f0a5-349">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="1f0a5-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1f0a5-350">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-351">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-351">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-352">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1f0a5-353">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-353">**New behavior**</span></span>

<span data-ttu-id="1f0a5-354">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1f0a5-355">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1f0a5-356">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-356">**Why**</span></span>

<span data-ttu-id="1f0a5-357">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1f0a5-358">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-358">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-359">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1f0a5-360">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1f0a5-361">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="1f0a5-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1f0a5-362">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="1f0a5-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1f0a5-363">Это изменение реализовано в EF Core 3.0, предварительная версия 5.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1f0a5-364">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-364">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-365">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1f0a5-366">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-366">**New behavior**</span></span>

<span data-ttu-id="1f0a5-367">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1f0a5-368">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-368">**Why**</span></span>

<span data-ttu-id="1f0a5-369">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1f0a5-370">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-370">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-371">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1f0a5-372">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="1f0a5-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1f0a5-373">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="1f0a5-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1f0a5-374">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-375">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-375">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-376">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1f0a5-377">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1f0a5-378">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-378">**New behavior**</span></span>

<span data-ttu-id="1f0a5-379">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1f0a5-380">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1f0a5-381">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-381">**Why**</span></span>

<span data-ttu-id="1f0a5-382">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1f0a5-383">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1f0a5-384">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1f0a5-385">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-385">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-386">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1f0a5-387">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1f0a5-388">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1f0a5-389">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1f0a5-390">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1f0a5-391">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="1f0a5-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1f0a5-392">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1f0a5-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1f0a5-393">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-394">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-394">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-395">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1f0a5-396">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-396">**New behavior**</span></span>

<span data-ttu-id="1f0a5-397">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1f0a5-398">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1f0a5-399">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1f0a5-400">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1f0a5-401">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-401">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="1f0a5-402">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1f0a5-403">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-403">**Why**</span></span>

<span data-ttu-id="1f0a5-404">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1f0a5-405">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1f0a5-406">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-406">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-407">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1f0a5-408">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="1f0a5-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1f0a5-409">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="1f0a5-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1f0a5-410">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-411">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-411">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-412">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-412">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="1f0a5-413">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1f0a5-414">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-414">**New behavior**</span></span>

<span data-ttu-id="1f0a5-415">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1f0a5-416">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1f0a5-417">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-417">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-418">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1f0a5-419">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1f0a5-420">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="1f0a5-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1f0a5-421">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="1f0a5-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1f0a5-422">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-423">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-423">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-424">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-424">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="1f0a5-425">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1f0a5-426">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-426">**New behavior**</span></span>

<span data-ttu-id="1f0a5-427">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1f0a5-428">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1f0a5-429">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-429">**Why**</span></span>

<span data-ttu-id="1f0a5-430">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1f0a5-431">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-431">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-432">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1f0a5-433">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1f0a5-434">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="1f0a5-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1f0a5-435">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="1f0a5-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1f0a5-436">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-437">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-437">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-438">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-438">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="1f0a5-439">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1f0a5-440">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-440">**New behavior**</span></span>

<span data-ttu-id="1f0a5-441">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1f0a5-442">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-442">**Why**</span></span>

<span data-ttu-id="1f0a5-443">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1f0a5-444">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-444">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-445">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1f0a5-446">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="1f0a5-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1f0a5-447">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="1f0a5-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1f0a5-448">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-449">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-449">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-450">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-450">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="1f0a5-451">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1f0a5-452">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1f0a5-453">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-453">**New behavior**</span></span>

<span data-ttu-id="1f0a5-454">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1f0a5-455">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1f0a5-456">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-456">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="1f0a5-457">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-457">**Why**</span></span>

<span data-ttu-id="1f0a5-458">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1f0a5-459">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-459">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-460">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1f0a5-461">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="1f0a5-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1f0a5-462">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="1f0a5-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1f0a5-463">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-464">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-464">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-465">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="1f0a5-466">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-466">**New behavior**</span></span>

<span data-ttu-id="1f0a5-467">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1f0a5-468">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-468">**Why**</span></span>

<span data-ttu-id="1f0a5-469">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1f0a5-470">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="1f0a5-471">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-471">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-472">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1f0a5-473">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="1f0a5-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1f0a5-474">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="1f0a5-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1f0a5-475">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-476">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-476">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-477">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1f0a5-478">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-478">**New behavior**</span></span>

<span data-ttu-id="1f0a5-479">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1f0a5-480">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1f0a5-481">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-481">**Why**</span></span>

<span data-ttu-id="1f0a5-482">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1f0a5-483">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-483">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-484">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1f0a5-485">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1f0a5-486">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="1f0a5-486">Backing fields are used by default</span></span>

[<span data-ttu-id="1f0a5-487">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="1f0a5-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1f0a5-488">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1f0a5-489">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-489">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-490">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1f0a5-491">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1f0a5-492">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-492">**New behavior**</span></span>

<span data-ttu-id="1f0a5-493">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1f0a5-494">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1f0a5-495">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-495">**Why**</span></span>

<span data-ttu-id="1f0a5-496">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1f0a5-497">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-497">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-498">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="1f0a5-499">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1f0a5-500">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="1f0a5-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1f0a5-501">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="1f0a5-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1f0a5-502">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-503">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-503">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-504">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1f0a5-505">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1f0a5-506">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-506">**New behavior**</span></span>

<span data-ttu-id="1f0a5-507">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1f0a5-508">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-508">**Why**</span></span>

<span data-ttu-id="1f0a5-509">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1f0a5-510">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-510">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-511">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1f0a5-512">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1f0a5-513">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="1f0a5-514">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-515">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-515">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-516">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе CLR, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="1f0a5-517">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-517">**New behavior**</span></span>

<span data-ttu-id="1f0a5-518">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1f0a5-519">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-519">**Why**</span></span>

<span data-ttu-id="1f0a5-520">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1f0a5-521">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-521">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-522">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1f0a5-523">В последующей предварительной версии EF Core 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1f0a5-524">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1f0a5-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1f0a5-525">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="1f0a5-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1f0a5-526">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-527">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-527">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-528">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1f0a5-529">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-529">**New behavior**</span></span>

<span data-ttu-id="1f0a5-530">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1f0a5-531">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-531">**Why**</span></span>

<span data-ttu-id="1f0a5-532">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="1f0a5-533">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1f0a5-534">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-534">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-535">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1f0a5-536">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="1f0a5-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1f0a5-537">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="1f0a5-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1f0a5-538">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-539">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-539">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-540">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1f0a5-541">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1f0a5-542">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-542">**New behavior**</span></span>

<span data-ttu-id="1f0a5-543">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1f0a5-544">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1f0a5-545">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1f0a5-546">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1f0a5-547">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-547">**Why**</span></span>

<span data-ttu-id="1f0a5-548">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1f0a5-549">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-549">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-550">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1f0a5-551">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1f0a5-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1f0a5-552">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="1f0a5-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1f0a5-553">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-554">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-554">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-555">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1f0a5-556">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1f0a5-557">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-557">**New behavior**</span></span>

<span data-ttu-id="1f0a5-558">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1f0a5-559">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-559">**Why**</span></span>

<span data-ttu-id="1f0a5-560">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1f0a5-561">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-561">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-562">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1f0a5-563">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1f0a5-564">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1f0a5-565">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="1f0a5-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1f0a5-566">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="1f0a5-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1f0a5-567">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-568">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-568">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-569">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1f0a5-570">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-570">**New behavior**</span></span>

<span data-ttu-id="1f0a5-571">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1f0a5-572">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-572">**Why**</span></span>

<span data-ttu-id="1f0a5-573">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1f0a5-574">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-574">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-575">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1f0a5-576">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-576">This isn't common.</span></span>
<span data-ttu-id="1f0a5-577">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1f0a5-578">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1f0a5-579">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="1f0a5-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1f0a5-580">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="1f0a5-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1f0a5-581">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-582">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-582">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-583">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1f0a5-584">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1f0a5-585">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1f0a5-586">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-586">**New behavior**</span></span>

<span data-ttu-id="1f0a5-587">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1f0a5-588">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1f0a5-589">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1f0a5-590">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1f0a5-591">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-591">**Why**</span></span>

<span data-ttu-id="1f0a5-592">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1f0a5-593">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-593">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-594">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1f0a5-595">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1f0a5-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1f0a5-596">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="1f0a5-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1f0a5-597">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-598">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-598">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-599">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1f0a5-600">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-600">**New behavior**</span></span>

<span data-ttu-id="1f0a5-601">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1f0a5-602">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-602">**Why**</span></span>

<span data-ttu-id="1f0a5-603">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1f0a5-604">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-604">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-605">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1f0a5-606">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1f0a5-607">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1f0a5-608">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="1f0a5-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1f0a5-609">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="1f0a5-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1f0a5-610">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-611">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-611">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-612">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="1f0a5-613">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1f0a5-614">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1f0a5-615">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1f0a5-616">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-616">**New behavior**</span></span>

<span data-ttu-id="1f0a5-617">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1f0a5-618">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-618">**Why**</span></span>

<span data-ttu-id="1f0a5-619">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1f0a5-620">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-620">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-621">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1f0a5-622">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-622">This is not common.</span></span>
<span data-ttu-id="1f0a5-623">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1f0a5-624">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1f0a5-625">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="1f0a5-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1f0a5-626">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="1f0a5-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1f0a5-627">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-628">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-628">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-629">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1f0a5-630">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="1f0a5-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1f0a5-631">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-631">**New behavior**</span></span>

<span data-ttu-id="1f0a5-632">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1f0a5-633">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-633">**Why**</span></span>

<span data-ttu-id="1f0a5-634">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1f0a5-635">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-635">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-636">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1f0a5-637">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1f0a5-638">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1f0a5-639">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1f0a5-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1f0a5-640">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="1f0a5-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1f0a5-641">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1f0a5-642">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-642">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-643">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1f0a5-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1f0a5-644">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-644">**New behavior**</span></span>

<span data-ttu-id="1f0a5-645">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1f0a5-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1f0a5-646">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-646">**Why**</span></span>

<span data-ttu-id="1f0a5-647">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1f0a5-648">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-648">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-649">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1f0a5-650">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1f0a5-651">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="1f0a5-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1f0a5-652">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="1f0a5-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1f0a5-653">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-654">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-654">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-655">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1f0a5-656">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-656">**New behavior**</span></span>

<span data-ttu-id="1f0a5-657">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1f0a5-658">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-658">**Why**</span></span>

<span data-ttu-id="1f0a5-659">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1f0a5-660">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1f0a5-661">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-661">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-662">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1f0a5-663">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="1f0a5-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1f0a5-664">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="1f0a5-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1f0a5-665">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-666">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-666">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-667">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1f0a5-668">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-668">**New behavior**</span></span>

<span data-ttu-id="1f0a5-669">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1f0a5-670">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1f0a5-671">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-671">**Why**</span></span>

<span data-ttu-id="1f0a5-672">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1f0a5-673">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-673">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-674">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="1f0a5-675">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="1f0a5-675">Metadata API changes</span></span>

[<span data-ttu-id="1f0a5-676">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="1f0a5-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1f0a5-677">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-678">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-678">**New behavior**</span></span>

<span data-ttu-id="1f0a5-679">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1f0a5-680">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-680">**Why**</span></span>

<span data-ttu-id="1f0a5-681">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1f0a5-682">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-682">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-683">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="1f0a5-684">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="1f0a5-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="1f0a5-685">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="1f0a5-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1f0a5-686">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1f0a5-687">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-687">**New behavior**</span></span>

<span data-ttu-id="1f0a5-688">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="1f0a5-689">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-689">**Why**</span></span>

<span data-ttu-id="1f0a5-690">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="1f0a5-691">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-691">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-692">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1f0a5-693">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="1f0a5-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1f0a5-694">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="1f0a5-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1f0a5-695">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1f0a5-696">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-696">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-697">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1f0a5-698">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-698">**New behavior**</span></span>

<span data-ttu-id="1f0a5-699">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1f0a5-700">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-700">**Why**</span></span>

<span data-ttu-id="1f0a5-701">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1f0a5-702">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-702">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-703">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1f0a5-704">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="1f0a5-705">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1f0a5-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1f0a5-706">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-706">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-707">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1f0a5-708">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-708">**New behavior**</span></span>

<span data-ttu-id="1f0a5-709">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1f0a5-710">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-710">**Why**</span></span>

<span data-ttu-id="1f0a5-711">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1f0a5-712">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-712">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-713">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1f0a5-714">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="1f0a5-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1f0a5-715">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="1f0a5-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1f0a5-716">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-717">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-717">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-718">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1f0a5-719">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-719">**New behavior**</span></span>

<span data-ttu-id="1f0a5-720">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="1f0a5-721">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-721">**Why**</span></span>

<span data-ttu-id="1f0a5-722">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1f0a5-723">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1f0a5-724">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-724">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-725">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="1f0a5-726">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1f0a5-727">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1f0a5-728">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="1f0a5-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1f0a5-729">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="1f0a5-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1f0a5-730">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-731">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-731">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-732">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1f0a5-733">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1f0a5-734">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-734">**New behavior**</span></span>

<span data-ttu-id="1f0a5-735">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="1f0a5-736">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-736">**Why**</span></span>

<span data-ttu-id="1f0a5-737">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1f0a5-738">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-738">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-739">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1f0a5-740">В EF Core можно и дальше использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1f0a5-741">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1f0a5-742">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="1f0a5-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1f0a5-743">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="1f0a5-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1f0a5-744">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-745">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-745">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-746">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="1f0a5-747">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-747">**New behavior**</span></span>

<span data-ttu-id="1f0a5-748">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1f0a5-749">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-749">**Why**</span></span>

<span data-ttu-id="1f0a5-750">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1f0a5-751">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1f0a5-752">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-752">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-753">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1f0a5-754">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1f0a5-755">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1f0a5-756">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="1f0a5-757">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="1f0a5-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="1f0a5-758">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="1f0a5-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="1f0a5-759">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1f0a5-760">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-760">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-761">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="1f0a5-762">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-762">**New behavior**</span></span>

<span data-ttu-id="1f0a5-763">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="1f0a5-764">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-764">**Why**</span></span>

<span data-ttu-id="1f0a5-765">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="1f0a5-766">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-766">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-767">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="1f0a5-768">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/aspnet/EntityFrameworkCore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="1f0a5-769">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="1f0a5-770">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="1f0a5-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1f0a5-771">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="1f0a5-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="1f0a5-772">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1f0a5-773">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-773">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-774">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="1f0a5-775">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-775">**New behavior**</span></span>

<span data-ttu-id="1f0a5-776">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="1f0a5-777">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-777">**Why**</span></span>

<span data-ttu-id="1f0a5-778">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="1f0a5-779">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="1f0a5-780">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-780">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-781">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="1f0a5-782">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1f0a5-783">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="1f0a5-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1f0a5-784">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="1f0a5-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1f0a5-785">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-786">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-786">**Change**</span></span>

<span data-ttu-id="1f0a5-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1f0a5-788">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-788">**Why**</span></span>

<span data-ttu-id="1f0a5-789">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1f0a5-790">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-790">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-791">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-791">Use the new name.</span></span> <span data-ttu-id="1f0a5-792">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="1f0a5-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1f0a5-793">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="1f0a5-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1f0a5-794">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="1f0a5-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1f0a5-795">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-796">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-796">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-797">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1f0a5-798">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1f0a5-799">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-799">**New behavior**</span></span>

<span data-ttu-id="1f0a5-800">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="1f0a5-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="1f0a5-801">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1f0a5-802">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-802">**Why**</span></span>

<span data-ttu-id="1f0a5-803">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1f0a5-804">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-804">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-805">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="1f0a5-806">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="1f0a5-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="1f0a5-807">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="1f0a5-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="1f0a5-808">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1f0a5-809">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-809">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-810">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="1f0a5-811">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-811">**New behavior**</span></span>

<span data-ttu-id="1f0a5-812">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="1f0a5-813">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-813">**Why**</span></span>

<span data-ttu-id="1f0a5-814">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="1f0a5-815">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="1f0a5-816">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-816">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-817">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="1f0a5-818">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="1f0a5-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="1f0a5-819">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="1f0a5-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="1f0a5-820">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1f0a5-821">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-821">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-822">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="1f0a5-823">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-823">**New behavior**</span></span>

<span data-ttu-id="1f0a5-824">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="1f0a5-825">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="1f0a5-826">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-826">**Why**</span></span>

<span data-ttu-id="1f0a5-827">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="1f0a5-828">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="1f0a5-829">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="1f0a5-830">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-830">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-831">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="1f0a5-832">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="1f0a5-833">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1f0a5-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="1f0a5-834">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="1f0a5-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="1f0a5-835">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1f0a5-836">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-836">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-837">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="1f0a5-838">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-838">**New behavior**</span></span>

<span data-ttu-id="1f0a5-839">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="1f0a5-840">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-840">**Why**</span></span>

<span data-ttu-id="1f0a5-841">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="1f0a5-842">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="1f0a5-843">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-843">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-844">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="1f0a5-845">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="1f0a5-846">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1f0a5-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="1f0a5-847">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="1f0a5-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="1f0a5-848">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1f0a5-849">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-849">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-850">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="1f0a5-851">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-851">**New behavior**</span></span>

<span data-ttu-id="1f0a5-852">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="1f0a5-853">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-853">**Why**</span></span>

<span data-ttu-id="1f0a5-854">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="1f0a5-855">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-855">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-856">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="1f0a5-857">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="1f0a5-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="1f0a5-858">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="1f0a5-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="1f0a5-859">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="1f0a5-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="1f0a5-860">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1f0a5-861">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-861">**Old behavior**</span></span>

<span data-ttu-id="1f0a5-862">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="1f0a5-863">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-863">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="1f0a5-864">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-864">**New behavior**</span></span>

<span data-ttu-id="1f0a5-865">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="1f0a5-866">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-866">**Why**</span></span>

<span data-ttu-id="1f0a5-867">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="1f0a5-868">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1f0a5-868">**Mitigations**</span></span>

<span data-ttu-id="1f0a5-869">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="1f0a5-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="1f0a5-870">Например:</span><span class="sxs-lookup"><span data-stu-id="1f0a5-870">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
