---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: b2e3881e3454377dab7851cba999ed6b891def4e
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812122"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="3f0db-102">Критические изменения в EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="3f0db-103">Указанные ниже изменения API и поведения могут нарушать работу существующих приложений при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="3f0db-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="3f0db-104">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="3f0db-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="3f0db-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="3f0db-105">Summary</span></span>

| <span data-ttu-id="3f0db-106">**Критические изменения**</span><span class="sxs-lookup"><span data-stu-id="3f0db-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="3f0db-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="3f0db-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="3f0db-108">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="3f0db-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="3f0db-109">Высокий</span><span class="sxs-lookup"><span data-stu-id="3f0db-109">High</span></span>       |
| [<span data-ttu-id="3f0db-110">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="3f0db-111">Высокий</span><span class="sxs-lookup"><span data-stu-id="3f0db-111">High</span></span>      |
| [<span data-ttu-id="3f0db-112">Программа командной строки EF Core "dotnet ef" больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f0db-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="3f0db-113">Высокий</span><span class="sxs-lookup"><span data-stu-id="3f0db-113">High</span></span>      |
| [<span data-ttu-id="3f0db-114">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="3f0db-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="3f0db-115">Высокий</span><span class="sxs-lookup"><span data-stu-id="3f0db-115">High</span></span>      |
| [<span data-ttu-id="3f0db-116">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="3f0db-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="3f0db-117">Высокий</span><span class="sxs-lookup"><span data-stu-id="3f0db-117">High</span></span>      |
| [<span data-ttu-id="3f0db-118">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="3f0db-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="3f0db-119">Высокий</span><span class="sxs-lookup"><span data-stu-id="3f0db-119">High</span></span>      |
| [<span data-ttu-id="3f0db-120">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f0db-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="3f0db-121">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-121">Medium</span></span>      |
| [<span data-ttu-id="3f0db-122">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="3f0db-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="3f0db-123">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-123">Medium</span></span>      |
| [<span data-ttu-id="3f0db-124">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="3f0db-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="3f0db-125">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-125">Medium</span></span>      |
| [<span data-ttu-id="3f0db-126">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="3f0db-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="3f0db-127">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-127">Medium</span></span>      |
| [<span data-ttu-id="3f0db-128">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="3f0db-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="3f0db-129">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-129">Medium</span></span>      |
| [<span data-ttu-id="3f0db-130">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="3f0db-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="3f0db-131">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-131">Medium</span></span>      |
| [<span data-ttu-id="3f0db-132">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="3f0db-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="3f0db-133">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-133">Medium</span></span>      |
| [<span data-ttu-id="3f0db-134">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="3f0db-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="3f0db-135">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-135">Medium</span></span>      |
| [<span data-ttu-id="3f0db-136">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="3f0db-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="3f0db-137">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-137">Medium</span></span>      |
| [<span data-ttu-id="3f0db-138">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="3f0db-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="3f0db-139">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-139">Medium</span></span>      |
| [<span data-ttu-id="3f0db-140">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="3f0db-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="3f0db-141">Средний</span><span class="sxs-lookup"><span data-stu-id="3f0db-141">Medium</span></span>      |
| [<span data-ttu-id="3f0db-142">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="3f0db-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="3f0db-143">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-143">Low</span></span>      |
| [<span data-ttu-id="3f0db-144">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="3f0db-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="3f0db-145">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-145">Low</span></span>      |
| [<span data-ttu-id="3f0db-146">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="3f0db-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="3f0db-147">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-147">Low</span></span>      |
| [<span data-ttu-id="3f0db-148">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="3f0db-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="3f0db-149">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-149">Low</span></span>      |
| [<span data-ttu-id="3f0db-150">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="3f0db-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="3f0db-151">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-151">Low</span></span>      |
| [<span data-ttu-id="3f0db-152">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="3f0db-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="3f0db-153">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-153">Low</span></span>      |
| [<span data-ttu-id="3f0db-154">Соглашение для свойства внешнего ключа больше не сопоставляет то же имя, что у свойства субъекта</span><span class="sxs-lookup"><span data-stu-id="3f0db-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="3f0db-155">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-155">Low</span></span>      |
| [<span data-ttu-id="3f0db-156">Подключение к базе данных теперь закрывается, если оно больше не используется, до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3f0db-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="3f0db-157">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-157">Low</span></span>      |
| [<span data-ttu-id="3f0db-158">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="3f0db-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="3f0db-159">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-159">Low</span></span>      |
| [<span data-ttu-id="3f0db-160">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="3f0db-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="3f0db-161">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-161">Low</span></span>      |
| [<span data-ttu-id="3f0db-162">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="3f0db-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="3f0db-163">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-163">Low</span></span>      |
| [<span data-ttu-id="3f0db-164">AddDbContext/AddDbContextPool больше не вызывает методы AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3f0db-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="3f0db-165">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-165">Low</span></span>      |
| [<span data-ttu-id="3f0db-166">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="3f0db-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="3f0db-167">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-167">Low</span></span>      |
| [<span data-ttu-id="3f0db-168">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="3f0db-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="3f0db-169">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-169">Low</span></span>      |
| [<span data-ttu-id="3f0db-170">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="3f0db-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="3f0db-171">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-171">Low</span></span>      |
| [<span data-ttu-id="3f0db-172">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="3f0db-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="3f0db-173">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-173">Low</span></span>      |
| [<span data-ttu-id="3f0db-174">Создание слишком многих внутренних поставщиков служб теперь по умолчанию является ошибкой</span><span class="sxs-lookup"><span data-stu-id="3f0db-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="3f0db-175">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-175">Low</span></span>      |
| [<span data-ttu-id="3f0db-176">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="3f0db-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="3f0db-177">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-177">Low</span></span>      |
| [<span data-ttu-id="3f0db-178">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="3f0db-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="3f0db-179">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-179">Low</span></span>      |
| [<span data-ttu-id="3f0db-180">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3f0db-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="3f0db-181">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-181">Low</span></span>      |
| [<span data-ttu-id="3f0db-182">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="3f0db-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="3f0db-183">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-183">Low</span></span>      |
| [<span data-ttu-id="3f0db-184">EF Core больше не отправляет PRAGMA для принудительного применения внешних ключей SQLite</span><span class="sxs-lookup"><span data-stu-id="3f0db-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="3f0db-185">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-185">Low</span></span>      |
| [<span data-ttu-id="3f0db-186">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3f0db-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="3f0db-187">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-187">Low</span></span>      |
| [<span data-ttu-id="3f0db-188">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="3f0db-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="3f0db-189">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-189">Low</span></span>      |
| [<span data-ttu-id="3f0db-190">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="3f0db-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="3f0db-191">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-191">Low</span></span>      |
| [<span data-ttu-id="3f0db-192">Идентификаторы миграции теперь создаются с использованием календаря инвариантных языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="3f0db-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="3f0db-193">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-193">Low</span></span>      |
| [<span data-ttu-id="3f0db-194">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3f0db-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="3f0db-195">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-195">Low</span></span>      |
| [<span data-ttu-id="3f0db-196">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="3f0db-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="3f0db-197">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-197">Low</span></span>      |
| [<span data-ttu-id="3f0db-198">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="3f0db-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="3f0db-199">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-199">Low</span></span>      |
| [<span data-ttu-id="3f0db-200">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="3f0db-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="3f0db-201">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-201">Low</span></span>      |
| [<span data-ttu-id="3f0db-202">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3f0db-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="3f0db-203">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-203">Low</span></span>      |
| [<span data-ttu-id="3f0db-204">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="3f0db-205">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-205">Low</span></span>      |
| [<span data-ttu-id="3f0db-206">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="3f0db-207">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-207">Low</span></span>      |
| [<span data-ttu-id="3f0db-208">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="3f0db-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="3f0db-209">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-209">Low</span></span>      |
| [<span data-ttu-id="3f0db-210">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="3f0db-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="3f0db-211">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-211">Low</span></span>      |
| [<span data-ttu-id="3f0db-212">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="3f0db-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="3f0db-213">Низкий</span><span class="sxs-lookup"><span data-stu-id="3f0db-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="3f0db-214">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="3f0db-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="3f0db-215">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="3f0db-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="3f0db-216">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-216">**Old behavior**</span></span>

<span data-ttu-id="3f0db-217">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="3f0db-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="3f0db-218">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="3f0db-219">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-219">**New behavior**</span></span>

<span data-ttu-id="3f0db-220">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="3f0db-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="3f0db-221">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="3f0db-222">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-222">**Why**</span></span>

<span data-ttu-id="3f0db-223">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="3f0db-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="3f0db-224">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="3f0db-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="3f0db-225">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="3f0db-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="3f0db-226">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="3f0db-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="3f0db-227">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="3f0db-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="3f0db-228">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="3f0db-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="3f0db-229">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-229">**Mitigations**</span></span>

<span data-ttu-id="3f0db-230">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="3f0db-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="3f0db-231">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="3f0db-232">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="3f0db-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="3f0db-233">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-233">**Old behavior**</span></span>

<span data-ttu-id="3f0db-234">До версии 3.0 EF Core был предназначен для .NET Standard 2.0 и выполнялся на всех платформах, поддерживающих этот стандарт, включая .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3f0db-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="3f0db-235">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-235">**New behavior**</span></span>

<span data-ttu-id="3f0db-236">Начиная с версии 3.0, EF Core предназначен для .NET Standard 2.1 и выполняется на всех платформах, поддерживающих этот стандарт.</span><span class="sxs-lookup"><span data-stu-id="3f0db-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="3f0db-237">Сюда не входит .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3f0db-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="3f0db-238">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-238">**Why**</span></span>

<span data-ttu-id="3f0db-239">Это часть стратегического решения по технологиям .NET, направленного на работу над .NET Core и другими современными платформами .NET, такими как Xamarin.</span><span class="sxs-lookup"><span data-stu-id="3f0db-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="3f0db-240">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-240">**Mitigations**</span></span>

<span data-ttu-id="3f0db-241">Рассмотрите возможность перехода на современные платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="3f0db-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="3f0db-242">Если это невозможно, продолжайте использовать EF Core 2.1 или EF Core 2.2, которые поддерживают .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3f0db-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="3f0db-243">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f0db-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="3f0db-244">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="3f0db-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="3f0db-245">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-245">**Old behavior**</span></span>

<span data-ttu-id="3f0db-246">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3f0db-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="3f0db-247">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-247">**New behavior**</span></span>

<span data-ttu-id="3f0db-248">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="3f0db-249">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-249">**Why**</span></span>

<span data-ttu-id="3f0db-250">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3f0db-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="3f0db-251">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="3f0db-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="3f0db-252">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="3f0db-253">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="3f0db-254">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-254">**Mitigations**</span></span>

<span data-ttu-id="3f0db-255">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="3f0db-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="3f0db-256">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f0db-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="3f0db-257">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="3f0db-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="3f0db-258">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-258">**Old behavior**</span></span>

<span data-ttu-id="3f0db-259">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="3f0db-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="3f0db-260">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-260">**New behavior**</span></span>

<span data-ttu-id="3f0db-261">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="3f0db-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="3f0db-262">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-262">**Why**</span></span>

<span data-ttu-id="3f0db-263">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="3f0db-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="3f0db-264">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-264">**Mitigations**</span></span>

<span data-ttu-id="3f0db-265">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="3f0db-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="3f0db-266">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="3f0db-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="3f0db-267">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="3f0db-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="3f0db-268">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="3f0db-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="3f0db-269">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-269">**Old behavior**</span></span>

<span data-ttu-id="3f0db-270">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="3f0db-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="3f0db-271">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-271">**New behavior**</span></span>

<span data-ttu-id="3f0db-272">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="3f0db-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="3f0db-273">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="3f0db-274">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="3f0db-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="3f0db-275">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="3f0db-276">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="3f0db-277">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-277">**Why**</span></span>

<span data-ttu-id="3f0db-278">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="3f0db-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="3f0db-279">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="3f0db-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="3f0db-280">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-280">**Mitigations**</span></span>

<span data-ttu-id="3f0db-281">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="3f0db-282">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="3f0db-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="3f0db-283">Отслеживание вопроса № 15392</span><span class="sxs-lookup"><span data-stu-id="3f0db-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="3f0db-284">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-284">**Old behavior**</span></span>

<span data-ttu-id="3f0db-285">До версии EF Core 3.0 метод FromSql пытался определить, можно ли выполнить составление для переданного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="3f0db-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="3f0db-286">Если SQL-запрос не допускал составления, как в случае с хранимой процедурой, вычисление производилось на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="3f0db-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="3f0db-287">В случае с приведенным ниже запросом хранимая процедура выполнялась на сервере, а метод FirstOrDefault — на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="3f0db-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="3f0db-288">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-288">**New behavior**</span></span>

<span data-ttu-id="3f0db-289">Начиная с версии 3.0 платформа EF Core не пытается анализировать код SQL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="3f0db-290">Поэтому если вы выполняете составление после метода FromSqlRaw или FromSqlInterpolated, EF Core составляет SQL-запрос, вызывая вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="3f0db-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="3f0db-291">При использовании хранимой процедуры с составлением вы получите исключение, вызванное недопустимым синтаксисом SQL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="3f0db-292">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-292">**Why**</span></span>

<span data-ttu-id="3f0db-293">EF Core 3.0 не поддерживает автоматическое вычисление на стороне клиента, так как оно было подвержено ошибкам, как описано [здесь](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="3f0db-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="3f0db-294">**Предотвращение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-294">**Mitigation**</span></span>

<span data-ttu-id="3f0db-295">Если вы используете хранимую процедуру в методе FromSqlRaw или FromSqlInterpolated, то знаете, что составление для нее невозможно, поэтому вы можете добавить __AsEnumerable или AsAsyncEnumerable__ сразу после вызова метода FromSql, чтобы избежать составления на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="3f0db-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="3f0db-296">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="3f0db-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="3f0db-297">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="3f0db-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="3f0db-298">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-298">**Old behavior**</span></span>

<span data-ttu-id="3f0db-299">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="3f0db-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="3f0db-300">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-300">**New behavior**</span></span>

<span data-ttu-id="3f0db-301">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="3f0db-302">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="3f0db-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="3f0db-303">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-303">**Why**</span></span>

<span data-ttu-id="3f0db-304">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f0db-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="3f0db-305">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-305">**Mitigations**</span></span>

<span data-ttu-id="3f0db-306">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="3f0db-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="3f0db-307">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="3f0db-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="3f0db-308">Отслеживание вопроса № 13518</span><span class="sxs-lookup"><span data-stu-id="3f0db-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="3f0db-309">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-309">**Old behavior**</span></span>

<span data-ttu-id="3f0db-310">До EF Core 3.0 один и тот же экземпляр сущности будет использоваться для каждого вхождения сущности с заданным типом и ИД.</span><span class="sxs-lookup"><span data-stu-id="3f0db-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="3f0db-311">Это соответствует поведению запросов отслеживания.</span><span class="sxs-lookup"><span data-stu-id="3f0db-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="3f0db-312">Например, запрос</span><span class="sxs-lookup"><span data-stu-id="3f0db-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="3f0db-313">возвращает тот же экземпляр `Category` для каждого `Product`, связанного с заданной категорией.</span><span class="sxs-lookup"><span data-stu-id="3f0db-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="3f0db-314">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-314">**New behavior**</span></span>

<span data-ttu-id="3f0db-315">Начиная с EF Core 3.0, при обнаружении сущности с заданным типом и ИД в разных местах возвращенного графа будут создаваться различные экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="3f0db-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="3f0db-316">Например, вышеприведенный запрос теперь будет возвращать новый экземпляр `Category` для каждого `Product`, даже если два продукта связаны с одной категорией.</span><span class="sxs-lookup"><span data-stu-id="3f0db-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="3f0db-317">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-317">**Why**</span></span>

<span data-ttu-id="3f0db-318">Разрешение идентификаторов (то есть определение того, что сущность имеет тот же тип и идентификатор, что и обнаруженная ранее сущность) добавляет дополнительную нагрузку на производительность и память.</span><span class="sxs-lookup"><span data-stu-id="3f0db-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="3f0db-319">Обычно это идет вразрез с причинами использования запросов без отслеживания.</span><span class="sxs-lookup"><span data-stu-id="3f0db-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="3f0db-320">Кроме того, хотя разрешение идентификаторов иногда может быть полезным, оно не требуется, если сущности должны быть сериализованы и отправлены клиенту, что является распространенным случаем для неотслеживаемых запросов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="3f0db-321">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-321">**Mitigations**</span></span>

<span data-ttu-id="3f0db-322">Используйте запрос с отслеживанием, если требуется разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="3f0db-323">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="3f0db-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="3f0db-324">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="3f0db-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="3f0db-325">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="3f0db-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="3f0db-326">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="3f0db-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="3f0db-327">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="3f0db-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="3f0db-328">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="3f0db-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="3f0db-329">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-329">**Old behavior**</span></span>

<span data-ttu-id="3f0db-330">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="3f0db-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="3f0db-331">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="3f0db-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="3f0db-332">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-332">**New behavior**</span></span>

<span data-ttu-id="3f0db-333">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="3f0db-334">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-334">**Why**</span></span>

<span data-ttu-id="3f0db-335">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="3f0db-336">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-336">**Mitigations**</span></span>

<span data-ttu-id="3f0db-337">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="3f0db-338">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3f0db-338">This can be avoided by:</span></span>
* <span data-ttu-id="3f0db-339">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="3f0db-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="3f0db-340">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="3f0db-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="3f0db-341">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="3f0db-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="3f0db-342">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="3f0db-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="3f0db-343">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="3f0db-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="3f0db-344">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="3f0db-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="3f0db-345">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-345">**Old behavior**</span></span>

<span data-ttu-id="3f0db-346">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="3f0db-347">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-347">**New behavior**</span></span>

<span data-ttu-id="3f0db-348">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="3f0db-349">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="3f0db-350">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="3f0db-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="3f0db-351">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-351">**Why**</span></span>

<span data-ttu-id="3f0db-352">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="3f0db-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="3f0db-353">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-353">**Mitigations**</span></span>

<span data-ttu-id="3f0db-354">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="3f0db-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="3f0db-355">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="3f0db-356">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="3f0db-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="3f0db-357">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="3f0db-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="3f0db-358">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="3f0db-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="3f0db-359">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="3f0db-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="3f0db-360">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-360">**Old behavior**</span></span>

<span data-ttu-id="3f0db-361">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3f0db-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="3f0db-362">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-362">**New behavior**</span></span>

<span data-ttu-id="3f0db-363">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="3f0db-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="3f0db-364">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="3f0db-365">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-365">**Why**</span></span>

<span data-ttu-id="3f0db-366">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="3f0db-367">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-367">**Mitigations**</span></span>

<span data-ttu-id="3f0db-368">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="3f0db-369">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="3f0db-370">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="3f0db-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="3f0db-371">Отслеживание вопроса № 18022</span><span class="sxs-lookup"><span data-stu-id="3f0db-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="3f0db-372">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-372">**Old behavior**</span></span>

<span data-ttu-id="3f0db-373">До версии 3.0 безотложная загрузка навигаций по коллекциям с помощью операторов `Include` приводила к созданию нескольких запросов к реляционной базе данных, по одной для каждого связанного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="3f0db-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="3f0db-374">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-374">**New behavior**</span></span>

<span data-ttu-id="3f0db-375">Начиная с версии 3.0 платформа EF Core создает один запрос с соединениями к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="3f0db-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="3f0db-376">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-376">**Why**</span></span>

<span data-ttu-id="3f0db-377">Отправка нескольких запросов для реализации одного запроса LINQ приводила ко множеству проблем, включая снижение производительности из-за необходимости нескольких циклов обращения к базе данных и проблем с согласованностью данных, так как при каждом запросе состояние базы данных могло быть разным.</span><span class="sxs-lookup"><span data-stu-id="3f0db-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="3f0db-378">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-378">**Mitigations**</span></span>

<span data-ttu-id="3f0db-379">Хотя технически это изменение не является критическим, оно может существенно повлиять на производительность приложения в случае, если один запрос содержит большое число операторов `Include` с навигациями по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="3f0db-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="3f0db-380">Дополнительные сведения и указания по повышению эффективности запросов см. в [этом комментарии](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085).</span><span class="sxs-lookup"><span data-stu-id="3f0db-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="3f0db-381">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="3f0db-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="3f0db-382">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="3f0db-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="3f0db-383">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-383">**Old behavior**</span></span>

<span data-ttu-id="3f0db-384">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="3f0db-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="3f0db-385">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-385">**New behavior**</span></span>

<span data-ttu-id="3f0db-386">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="3f0db-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="3f0db-387">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-387">**Why**</span></span>

<span data-ttu-id="3f0db-388">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="3f0db-389">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-389">**Mitigations**</span></span>

<span data-ttu-id="3f0db-390">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="3f0db-391">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="3f0db-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="3f0db-392">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="3f0db-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="3f0db-393">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-393">**Old behavior**</span></span>

<span data-ttu-id="3f0db-394">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/keyless-entity-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="3f0db-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="3f0db-395">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="3f0db-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="3f0db-396">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-396">**New behavior**</span></span>

<span data-ttu-id="3f0db-397">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="3f0db-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="3f0db-398">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="3f0db-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="3f0db-399">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-399">**Why**</span></span>

<span data-ttu-id="3f0db-400">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="3f0db-401">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="3f0db-402">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="3f0db-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="3f0db-403">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-403">**Mitigations**</span></span>

<span data-ttu-id="3f0db-404">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="3f0db-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="3f0db-405">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="3f0db-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="3f0db-406">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="3f0db-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="3f0db-407">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="3f0db-408">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="3f0db-409">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="3f0db-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="3f0db-410">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="3f0db-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="3f0db-411">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-411">**Old behavior**</span></span>

<span data-ttu-id="3f0db-412">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="3f0db-413">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-413">**New behavior**</span></span>

<span data-ttu-id="3f0db-414">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="3f0db-415">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="3f0db-416">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="3f0db-417">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="3f0db-418">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-418">For example:</span></span>

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

<span data-ttu-id="3f0db-419">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="3f0db-420">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-420">**Why**</span></span>

<span data-ttu-id="3f0db-421">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="3f0db-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="3f0db-422">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="3f0db-423">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-423">**Mitigations**</span></span>

<span data-ttu-id="3f0db-424">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="3f0db-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="3f0db-425">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="3f0db-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="3f0db-426">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="3f0db-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="3f0db-427">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-427">**Old behavior**</span></span>

<span data-ttu-id="3f0db-428">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="3f0db-428">Consider the following model:</span></span>
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
<span data-ttu-id="3f0db-429">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="3f0db-430">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-430">**New behavior**</span></span>

<span data-ttu-id="3f0db-431">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="3f0db-432">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="3f0db-433">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-433">**Mitigations**</span></span>

<span data-ttu-id="3f0db-434">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="3f0db-435">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="3f0db-436">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="3f0db-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="3f0db-437">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="3f0db-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="3f0db-438">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-438">**Old behavior**</span></span>

<span data-ttu-id="3f0db-439">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="3f0db-439">Consider the following model:</span></span>
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
<span data-ttu-id="3f0db-440">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="3f0db-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="3f0db-441">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-441">**New behavior**</span></span>

<span data-ttu-id="3f0db-442">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="3f0db-443">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="3f0db-444">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-444">**Why**</span></span>

<span data-ttu-id="3f0db-445">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="3f0db-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="3f0db-446">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-446">**Mitigations**</span></span>

<span data-ttu-id="3f0db-447">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="3f0db-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="3f0db-448">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="3f0db-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="3f0db-449">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="3f0db-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="3f0db-450">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="3f0db-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="3f0db-451">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-451">**Old behavior**</span></span>

<span data-ttu-id="3f0db-452">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="3f0db-452">Consider the following model:</span></span>
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

<span data-ttu-id="3f0db-453">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f0db-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="3f0db-454">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-454">**New behavior**</span></span>

<span data-ttu-id="3f0db-455">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="3f0db-456">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-456">**Why**</span></span>

<span data-ttu-id="3f0db-457">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="3f0db-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="3f0db-458">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-458">**Mitigations**</span></span>

<span data-ttu-id="3f0db-459">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="3f0db-460">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="3f0db-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="3f0db-461">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="3f0db-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="3f0db-462">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-462">**Old behavior**</span></span>

<span data-ttu-id="3f0db-463">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="3f0db-463">Consider the following model:</span></span>
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
<span data-ttu-id="3f0db-464">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="3f0db-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="3f0db-465">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="3f0db-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="3f0db-466">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-466">**New behavior**</span></span>

<span data-ttu-id="3f0db-467">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="3f0db-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="3f0db-468">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="3f0db-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="3f0db-469">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-469">For example:</span></span>

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

<span data-ttu-id="3f0db-470">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-470">**Why**</span></span>

<span data-ttu-id="3f0db-471">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="3f0db-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="3f0db-472">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-472">**Mitigations**</span></span>

<span data-ttu-id="3f0db-473">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="3f0db-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="3f0db-474">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3f0db-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="3f0db-475">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="3f0db-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="3f0db-476">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-476">**Old behavior**</span></span>

<span data-ttu-id="3f0db-477">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="3f0db-478">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-478">**New behavior**</span></span>

<span data-ttu-id="3f0db-479">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="3f0db-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="3f0db-480">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-480">**Why**</span></span>

<span data-ttu-id="3f0db-481">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="3f0db-482">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="3f0db-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="3f0db-483">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-483">**Mitigations**</span></span>

<span data-ttu-id="3f0db-484">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="3f0db-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="3f0db-485">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="3f0db-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="3f0db-486">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="3f0db-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="3f0db-487">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-487">**Old behavior**</span></span>

<span data-ttu-id="3f0db-488">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="3f0db-489">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-489">**New behavior**</span></span>

<span data-ttu-id="3f0db-490">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="3f0db-491">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="3f0db-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="3f0db-492">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-492">**Why**</span></span>

<span data-ttu-id="3f0db-493">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="3f0db-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="3f0db-494">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-494">**Mitigations**</span></span>

<span data-ttu-id="3f0db-495">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="3f0db-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="3f0db-496">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="3f0db-497">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="3f0db-497">Backing fields are used by default</span></span>

[<span data-ttu-id="3f0db-498">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="3f0db-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="3f0db-499">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-499">**Old behavior**</span></span>

<span data-ttu-id="3f0db-500">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="3f0db-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="3f0db-501">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="3f0db-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="3f0db-502">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-502">**New behavior**</span></span>

<span data-ttu-id="3f0db-503">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="3f0db-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="3f0db-504">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="3f0db-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="3f0db-505">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-505">**Why**</span></span>

<span data-ttu-id="3f0db-506">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="3f0db-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="3f0db-507">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-507">**Mitigations**</span></span>

<span data-ttu-id="3f0db-508">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="3f0db-509">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="3f0db-510">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="3f0db-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="3f0db-511">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="3f0db-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="3f0db-512">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-512">**Old behavior**</span></span>

<span data-ttu-id="3f0db-513">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="3f0db-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="3f0db-514">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="3f0db-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="3f0db-515">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-515">**New behavior**</span></span>

<span data-ttu-id="3f0db-516">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="3f0db-517">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-517">**Why**</span></span>

<span data-ttu-id="3f0db-518">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="3f0db-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="3f0db-519">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-519">**Mitigations**</span></span>

<span data-ttu-id="3f0db-520">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="3f0db-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="3f0db-521">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="3f0db-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="3f0db-522">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="3f0db-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="3f0db-523">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-523">**Old behavior**</span></span>

<span data-ttu-id="3f0db-524">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе .NET, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="3f0db-525">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-525">**New behavior**</span></span>

<span data-ttu-id="3f0db-526">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="3f0db-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="3f0db-527">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-527">**Why**</span></span>

<span data-ttu-id="3f0db-528">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="3f0db-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="3f0db-529">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-529">**Mitigations**</span></span>

<span data-ttu-id="3f0db-530">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="3f0db-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="3f0db-531">В будущем выпуске EF Core после версии 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства (см. вопрос [№ 15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="3f0db-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="3f0db-532">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3f0db-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="3f0db-533">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="3f0db-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="3f0db-534">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-534">**Old behavior**</span></span>

<span data-ttu-id="3f0db-535">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3f0db-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="3f0db-536">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-536">**New behavior**</span></span>

<span data-ttu-id="3f0db-537">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3f0db-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="3f0db-538">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-538">**Why**</span></span>

<span data-ttu-id="3f0db-539">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="3f0db-540">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="3f0db-541">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-541">**Mitigations**</span></span>

<span data-ttu-id="3f0db-542">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3f0db-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="3f0db-543">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="3f0db-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="3f0db-544">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="3f0db-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="3f0db-545">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-545">**Old behavior**</span></span>

<span data-ttu-id="3f0db-546">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="3f0db-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="3f0db-547">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="3f0db-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="3f0db-548">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-548">**New behavior**</span></span>

<span data-ttu-id="3f0db-549">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="3f0db-550">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="3f0db-551">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="3f0db-552">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="3f0db-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="3f0db-553">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-553">**Why**</span></span>

<span data-ttu-id="3f0db-554">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="3f0db-555">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-555">**Mitigations**</span></span>

<span data-ttu-id="3f0db-556">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="3f0db-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="3f0db-557">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="3f0db-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="3f0db-558">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="3f0db-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="3f0db-559">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-559">**Old behavior**</span></span>

<span data-ttu-id="3f0db-560">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="3f0db-561">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="3f0db-562">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-562">**New behavior**</span></span>

<span data-ttu-id="3f0db-563">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="3f0db-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="3f0db-564">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-564">**Why**</span></span>

<span data-ttu-id="3f0db-565">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="3f0db-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="3f0db-566">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-566">**Mitigations**</span></span>

<span data-ttu-id="3f0db-567">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="3f0db-568">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="3f0db-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="3f0db-569">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="3f0db-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="3f0db-570">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="3f0db-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="3f0db-571">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="3f0db-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="3f0db-572">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-572">**Old behavior**</span></span>

<span data-ttu-id="3f0db-573">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="3f0db-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="3f0db-574">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-574">**New behavior**</span></span>

<span data-ttu-id="3f0db-575">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="3f0db-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="3f0db-576">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-576">**Why**</span></span>

<span data-ttu-id="3f0db-577">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="3f0db-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="3f0db-578">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-578">**Mitigations**</span></span>

<span data-ttu-id="3f0db-579">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="3f0db-580">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="3f0db-580">This isn't common.</span></span>
<span data-ttu-id="3f0db-581">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="3f0db-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="3f0db-582">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="3f0db-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="3f0db-583">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="3f0db-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="3f0db-584">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="3f0db-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="3f0db-585">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-585">**Old behavior**</span></span>

<span data-ttu-id="3f0db-586">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="3f0db-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="3f0db-587">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="3f0db-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="3f0db-588">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="3f0db-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="3f0db-589">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-589">**New behavior**</span></span>

<span data-ttu-id="3f0db-590">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="3f0db-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="3f0db-591">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="3f0db-592">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="3f0db-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="3f0db-593">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="3f0db-594">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-594">**Why**</span></span>

<span data-ttu-id="3f0db-595">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="3f0db-596">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-596">**Mitigations**</span></span>

<span data-ttu-id="3f0db-597">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="3f0db-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="3f0db-598">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="3f0db-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="3f0db-599">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="3f0db-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="3f0db-600">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-600">**Old behavior**</span></span>

<span data-ttu-id="3f0db-601">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="3f0db-602">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-602">**New behavior**</span></span>

<span data-ttu-id="3f0db-603">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3f0db-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="3f0db-604">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-604">**Why**</span></span>

<span data-ttu-id="3f0db-605">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="3f0db-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="3f0db-606">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-606">**Mitigations**</span></span>

<span data-ttu-id="3f0db-607">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="3f0db-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="3f0db-608">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="3f0db-609">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="3f0db-610">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="3f0db-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="3f0db-611">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="3f0db-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="3f0db-612">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-612">**Old behavior**</span></span>

<span data-ttu-id="3f0db-613">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="3f0db-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="3f0db-614">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="3f0db-615">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="3f0db-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="3f0db-616">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3f0db-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="3f0db-617">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-617">**New behavior**</span></span>

<span data-ttu-id="3f0db-618">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="3f0db-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="3f0db-619">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-619">**Why**</span></span>

<span data-ttu-id="3f0db-620">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="3f0db-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="3f0db-621">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-621">**Mitigations**</span></span>

<span data-ttu-id="3f0db-622">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="3f0db-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="3f0db-623">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="3f0db-623">This is not common.</span></span>
<span data-ttu-id="3f0db-624">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3f0db-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="3f0db-625">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="3f0db-626">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="3f0db-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="3f0db-627">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="3f0db-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="3f0db-628">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-628">**Old behavior**</span></span>

<span data-ttu-id="3f0db-629">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="3f0db-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="3f0db-630">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="3f0db-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="3f0db-631">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-631">**New behavior**</span></span>

<span data-ttu-id="3f0db-632">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="3f0db-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="3f0db-633">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-633">**Why**</span></span>

<span data-ttu-id="3f0db-634">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="3f0db-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="3f0db-635">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-635">**Mitigations**</span></span>

<span data-ttu-id="3f0db-636">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="3f0db-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="3f0db-637">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="3f0db-638">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="3f0db-639">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3f0db-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="3f0db-640">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="3f0db-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="3f0db-641">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-641">**Old behavior**</span></span>

<span data-ttu-id="3f0db-642">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="3f0db-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="3f0db-643">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-643">**New behavior**</span></span>

<span data-ttu-id="3f0db-644">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="3f0db-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="3f0db-645">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-645">**Why**</span></span>

<span data-ttu-id="3f0db-646">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="3f0db-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="3f0db-647">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-647">**Mitigations**</span></span>

<span data-ttu-id="3f0db-648">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="3f0db-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="3f0db-649">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="3f0db-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="3f0db-650">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="3f0db-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="3f0db-651">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="3f0db-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="3f0db-652">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-652">**Old behavior**</span></span>

<span data-ttu-id="3f0db-653">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="3f0db-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="3f0db-654">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-654">**New behavior**</span></span>

<span data-ttu-id="3f0db-655">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="3f0db-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="3f0db-656">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-656">**Why**</span></span>

<span data-ttu-id="3f0db-657">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="3f0db-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="3f0db-658">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="3f0db-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="3f0db-659">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-659">**Mitigations**</span></span>

<span data-ttu-id="3f0db-660">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="3f0db-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="3f0db-661">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="3f0db-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="3f0db-662">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="3f0db-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="3f0db-663">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-663">**Old behavior**</span></span>

<span data-ttu-id="3f0db-664">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="3f0db-665">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-665">**New behavior**</span></span>

<span data-ttu-id="3f0db-666">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="3f0db-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="3f0db-667">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="3f0db-668">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-668">**Why**</span></span>

<span data-ttu-id="3f0db-669">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="3f0db-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="3f0db-670">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-670">**Mitigations**</span></span>

<span data-ttu-id="3f0db-671">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="3f0db-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="3f0db-672">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="3f0db-672">Metadata API changes</span></span>

[<span data-ttu-id="3f0db-673">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="3f0db-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3f0db-674">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-674">**New behavior**</span></span>

<span data-ttu-id="3f0db-675">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="3f0db-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="3f0db-676">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-676">**Why**</span></span>

<span data-ttu-id="3f0db-677">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="3f0db-678">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-678">**Mitigations**</span></span>

<span data-ttu-id="3f0db-679">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="3f0db-680">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="3f0db-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="3f0db-681">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="3f0db-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3f0db-682">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-682">**New behavior**</span></span>

<span data-ttu-id="3f0db-683">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="3f0db-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="3f0db-684">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-684">**Why**</span></span>

<span data-ttu-id="3f0db-685">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="3f0db-686">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-686">**Mitigations**</span></span>

<span data-ttu-id="3f0db-687">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="3f0db-688">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="3f0db-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="3f0db-689">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="3f0db-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="3f0db-690">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-690">**Old behavior**</span></span>

<span data-ttu-id="3f0db-691">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="3f0db-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3f0db-692">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-692">**New behavior**</span></span>

<span data-ttu-id="3f0db-693">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="3f0db-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3f0db-694">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-694">**Why**</span></span>

<span data-ttu-id="3f0db-695">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="3f0db-696">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-696">**Mitigations**</span></span>

<span data-ttu-id="3f0db-697">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="3f0db-698">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="3f0db-699">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3f0db-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="3f0db-700">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-700">**Old behavior**</span></span>

<span data-ttu-id="3f0db-701">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="3f0db-702">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-702">**New behavior**</span></span>

<span data-ttu-id="3f0db-703">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="3f0db-704">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-704">**Why**</span></span>

<span data-ttu-id="3f0db-705">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="3f0db-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="3f0db-706">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-706">**Mitigations**</span></span>

<span data-ttu-id="3f0db-707">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3f0db-708">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="3f0db-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3f0db-709">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="3f0db-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="3f0db-710">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-710">**Old behavior**</span></span>

<span data-ttu-id="3f0db-711">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="3f0db-712">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-712">**New behavior**</span></span>

<span data-ttu-id="3f0db-713">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="3f0db-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="3f0db-714">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-714">**Why**</span></span>

<span data-ttu-id="3f0db-715">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="3f0db-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="3f0db-716">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="3f0db-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3f0db-717">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-717">**Mitigations**</span></span>

<span data-ttu-id="3f0db-718">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="3f0db-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="3f0db-719">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="3f0db-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="3f0db-720">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="3f0db-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3f0db-721">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="3f0db-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3f0db-722">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="3f0db-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="3f0db-723">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-723">**Old behavior**</span></span>

<span data-ttu-id="3f0db-724">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="3f0db-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="3f0db-725">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="3f0db-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="3f0db-726">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-726">**New behavior**</span></span>

<span data-ttu-id="3f0db-727">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="3f0db-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="3f0db-728">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-728">**Why**</span></span>

<span data-ttu-id="3f0db-729">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="3f0db-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3f0db-730">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-730">**Mitigations**</span></span>

<span data-ttu-id="3f0db-731">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="3f0db-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="3f0db-732">В EF Core можно и дальше использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="3f0db-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="3f0db-733">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="3f0db-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="3f0db-734">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="3f0db-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="3f0db-735">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="3f0db-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="3f0db-736">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-736">**Old behavior**</span></span>

<span data-ttu-id="3f0db-737">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="3f0db-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="3f0db-738">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-738">**New behavior**</span></span>

<span data-ttu-id="3f0db-739">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="3f0db-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="3f0db-740">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-740">**Why**</span></span>

<span data-ttu-id="3f0db-741">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="3f0db-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="3f0db-742">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="3f0db-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="3f0db-743">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-743">**Mitigations**</span></span>

<span data-ttu-id="3f0db-744">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="3f0db-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="3f0db-745">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="3f0db-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="3f0db-746">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="3f0db-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="3f0db-747">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="3f0db-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="3f0db-748">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="3f0db-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="3f0db-749">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="3f0db-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="3f0db-750">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-750">**Old behavior**</span></span>

<span data-ttu-id="3f0db-751">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="3f0db-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="3f0db-752">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-752">**New behavior**</span></span>

<span data-ttu-id="3f0db-753">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3f0db-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="3f0db-754">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-754">**Why**</span></span>

<span data-ttu-id="3f0db-755">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="3f0db-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="3f0db-756">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-756">**Mitigations**</span></span>

<span data-ttu-id="3f0db-757">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="3f0db-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="3f0db-758">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/aspnet/EntityFrameworkCore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="3f0db-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="3f0db-759">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="3f0db-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="3f0db-760">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3f0db-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="3f0db-761">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="3f0db-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="3f0db-762">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-762">**Old behavior**</span></span>

<span data-ttu-id="3f0db-763">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="3f0db-764">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-764">**New behavior**</span></span>

<span data-ttu-id="3f0db-765">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="3f0db-766">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-766">**Why**</span></span>

<span data-ttu-id="3f0db-767">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="3f0db-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="3f0db-768">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="3f0db-769">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-769">**Mitigations**</span></span>

<span data-ttu-id="3f0db-770">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="3f0db-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="3f0db-771">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="3f0db-772">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="3f0db-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="3f0db-773">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="3f0db-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="3f0db-774">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-774">**Change**</span></span>

<span data-ttu-id="3f0db-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="3f0db-776">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-776">**Why**</span></span>

<span data-ttu-id="3f0db-777">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="3f0db-778">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-778">**Mitigations**</span></span>

<span data-ttu-id="3f0db-779">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="3f0db-779">Use the new name.</span></span> <span data-ttu-id="3f0db-780">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="3f0db-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="3f0db-781">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="3f0db-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="3f0db-782">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="3f0db-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="3f0db-783">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-783">**Old behavior**</span></span>

<span data-ttu-id="3f0db-784">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="3f0db-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="3f0db-785">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="3f0db-786">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-786">**New behavior**</span></span>

<span data-ttu-id="3f0db-787">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="3f0db-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="3f0db-788">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="3f0db-789">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-789">**Why**</span></span>

<span data-ttu-id="3f0db-790">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="3f0db-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="3f0db-791">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-791">**Mitigations**</span></span>

<span data-ttu-id="3f0db-792">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="3f0db-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="3f0db-793">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="3f0db-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="3f0db-794">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="3f0db-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="3f0db-795">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-795">**Old behavior**</span></span>

<span data-ttu-id="3f0db-796">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="3f0db-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="3f0db-797">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-797">**New behavior**</span></span>

<span data-ttu-id="3f0db-798">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="3f0db-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="3f0db-799">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-799">**Why**</span></span>

<span data-ttu-id="3f0db-800">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="3f0db-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="3f0db-801">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="3f0db-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="3f0db-802">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-802">**Mitigations**</span></span>

<span data-ttu-id="3f0db-803">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="3f0db-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="3f0db-804">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3f0db-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="3f0db-805">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="3f0db-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="3f0db-806">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-806">**Old behavior**</span></span>

<span data-ttu-id="3f0db-807">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="3f0db-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="3f0db-808">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-808">**New behavior**</span></span>

<span data-ttu-id="3f0db-809">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="3f0db-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="3f0db-810">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="3f0db-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="3f0db-811">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-811">**Why**</span></span>

<span data-ttu-id="3f0db-812">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="3f0db-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="3f0db-813">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="3f0db-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="3f0db-814">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="3f0db-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="3f0db-815">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-815">**Mitigations**</span></span>

<span data-ttu-id="3f0db-816">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="3f0db-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="3f0db-817">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="3f0db-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="3f0db-818">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="3f0db-819">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="3f0db-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="3f0db-820">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-820">**Old behavior**</span></span>

<span data-ttu-id="3f0db-821">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="3f0db-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="3f0db-822">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-822">**New behavior**</span></span>

<span data-ttu-id="3f0db-823">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3f0db-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3f0db-824">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-824">**Why**</span></span>

<span data-ttu-id="3f0db-825">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="3f0db-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="3f0db-826">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="3f0db-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="3f0db-827">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-827">**Mitigations**</span></span>

<span data-ttu-id="3f0db-828">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3f0db-829">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="3f0db-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="3f0db-830">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3f0db-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="3f0db-831">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="3f0db-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="3f0db-832">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-832">**Old behavior**</span></span>

<span data-ttu-id="3f0db-833">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="3f0db-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="3f0db-834">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-834">**New behavior**</span></span>

<span data-ttu-id="3f0db-835">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3f0db-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3f0db-836">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-836">**Why**</span></span>

<span data-ttu-id="3f0db-837">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="3f0db-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="3f0db-838">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-838">**Mitigations**</span></span>

<span data-ttu-id="3f0db-839">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="3f0db-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3f0db-840">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="3f0db-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="3f0db-841">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="3f0db-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="3f0db-842">Отслеживание вопроса № 15636</span><span class="sxs-lookup"><span data-stu-id="3f0db-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="3f0db-843">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-843">**Old behavior**</span></span>

<span data-ttu-id="3f0db-844">Пакет Microsoft.EntityFrameworkCore.SqlServer ранее зависел от System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3f0db-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="3f0db-845">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-845">**New behavior**</span></span>

<span data-ttu-id="3f0db-846">Мы обновили пакет, и теперь он зависит от Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3f0db-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="3f0db-847">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-847">**Why**</span></span>

<span data-ttu-id="3f0db-848">Microsoft.Data.SqlClient теперь является основным разрабатываемым драйвером для доступа к SQL Server вместо System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3f0db-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="3f0db-849">Некоторые важные функции, такие как Always Encrypted, доступны только в Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3f0db-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="3f0db-850">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-850">**Mitigations**</span></span>

<span data-ttu-id="3f0db-851">Если код включает прямую зависимость от System.Data.SqlClient, необходимо изменить код так, чтобы он ссылался на Microsoft.Data.SqlClient. Поскольку API двух пакетов очень хорошо совместимы, достаточно изменить пакет и пространство имен.</span><span class="sxs-lookup"><span data-stu-id="3f0db-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="3f0db-852">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="3f0db-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="3f0db-853">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="3f0db-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="3f0db-854">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-854">**Old behavior**</span></span>

<span data-ttu-id="3f0db-855">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="3f0db-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="3f0db-856">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-856">For example:</span></span>

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

<span data-ttu-id="3f0db-857">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-857">**New behavior**</span></span>

<span data-ttu-id="3f0db-858">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="3f0db-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="3f0db-859">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-859">**Why**</span></span>

<span data-ttu-id="3f0db-860">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="3f0db-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="3f0db-861">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-861">**Mitigations**</span></span>

<span data-ttu-id="3f0db-862">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="3f0db-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="3f0db-863">Например:</span><span class="sxs-lookup"><span data-stu-id="3f0db-863">For example:</span></span>

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

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="3f0db-864">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="3f0db-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="3f0db-865">Отслеживание вопроса № 12757</span><span class="sxs-lookup"><span data-stu-id="3f0db-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="3f0db-866">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-866">**Old behavior**</span></span>

<span data-ttu-id="3f0db-867">DbFunction, настроенная со схемой в виде пустой строки, обрабатывалась как встроенная функция без схемы.</span><span class="sxs-lookup"><span data-stu-id="3f0db-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="3f0db-868">Например, следующий код сопоставит функцию CLR `DatePart` со встроенной функцией `DATEPART` в SqlServer.</span><span class="sxs-lookup"><span data-stu-id="3f0db-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="3f0db-869">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="3f0db-869">**New behavior**</span></span>

<span data-ttu-id="3f0db-870">Все сопоставления DbFunction считаются сопоставленными с определяемыми пользователем функциями.</span><span class="sxs-lookup"><span data-stu-id="3f0db-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="3f0db-871">Поэтому пустое строковое значение поместит функцию в схему по умолчанию для модели.</span><span class="sxs-lookup"><span data-stu-id="3f0db-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="3f0db-872">Это может быть схема, настроенная явным образом с помощью `modelBuilder.HasDefaultSchema()` текучего API либо `dbo`.</span><span class="sxs-lookup"><span data-stu-id="3f0db-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="3f0db-873">**Причина**</span><span class="sxs-lookup"><span data-stu-id="3f0db-873">**Why**</span></span>

<span data-ttu-id="3f0db-874">Ранее эта функция считалась встроенной из-за пустой схемы, но эта логика применима только к SqlServer, где встроенные функции не принадлежат ни одной схеме.</span><span class="sxs-lookup"><span data-stu-id="3f0db-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="3f0db-875">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="3f0db-875">**Mitigations**</span></span>

<span data-ttu-id="3f0db-876">Вручную настройте преобразование функции DbFunction, чтобы сопоставить ее со встроенной функцией.</span><span class="sxs-lookup"><span data-stu-id="3f0db-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
