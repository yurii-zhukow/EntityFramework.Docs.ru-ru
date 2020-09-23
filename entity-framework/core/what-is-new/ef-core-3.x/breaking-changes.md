---
title: Критические изменения в EF Core 3.x — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 3.x
author: ajcvickers
ms.date: 09/05/2020
uid: core/what-is-new/ef-core-3.x/breaking-changes
ms.openlocfilehash: e348cb630d91ebe4536b73b9a7bd9a7b6a46db79
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072243"
---
# <a name="breaking-changes-included-in-ef-core-3x"></a><span data-ttu-id="bfda4-103">Критические изменения в EF Core 3.x</span><span class="sxs-lookup"><span data-stu-id="bfda4-103">Breaking changes included in EF Core 3.x</span></span>

<span data-ttu-id="bfda4-104">Указанные ниже изменения API и поведения могут нарушать работу существующих приложений при их обновлении до версии 3.x.</span><span class="sxs-lookup"><span data-stu-id="bfda4-104">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.x.</span></span>
<span data-ttu-id="bfda4-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="bfda4-105">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="bfda4-106">Сводка</span><span class="sxs-lookup"><span data-stu-id="bfda4-106">Summary</span></span>

| <span data-ttu-id="bfda4-107">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="bfda4-108">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="bfda4-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="bfda4-109">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="bfda4-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="bfda4-110">Высокий</span><span class="sxs-lookup"><span data-stu-id="bfda4-110">High</span></span>       |
| [<span data-ttu-id="bfda4-111">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="bfda4-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="bfda4-112">Высокий</span><span class="sxs-lookup"><span data-stu-id="bfda4-112">High</span></span>      |
| [<span data-ttu-id="bfda4-113">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="bfda4-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="bfda4-114">Высокий</span><span class="sxs-lookup"><span data-stu-id="bfda4-114">High</span></span>      |
| [<span data-ttu-id="bfda4-115">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="bfda4-115">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="bfda4-116">Высокий</span><span class="sxs-lookup"><span data-stu-id="bfda4-116">High</span></span>      |
| [<span data-ttu-id="bfda4-117">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="bfda4-117">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="bfda4-118">Высокий</span><span class="sxs-lookup"><span data-stu-id="bfda4-118">High</span></span>      |
| [<span data-ttu-id="bfda4-119">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="bfda4-119">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="bfda4-120">Высокий</span><span class="sxs-lookup"><span data-stu-id="bfda4-120">High</span></span>      |
| [<span data-ttu-id="bfda4-121">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfda4-121">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="bfda4-122">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-122">Medium</span></span>      |
| [<span data-ttu-id="bfda4-123">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="bfda4-123">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="bfda4-124">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-124">Medium</span></span>      |
| [<span data-ttu-id="bfda4-125">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="bfda4-125">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="bfda4-126">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-126">Medium</span></span>      |
| [<span data-ttu-id="bfda4-127">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="bfda4-127">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="bfda4-128">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-128">Medium</span></span>      |
| [<span data-ttu-id="bfda4-129">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="bfda4-129">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="bfda4-130">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-130">Medium</span></span>      |
| [<span data-ttu-id="bfda4-131">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="bfda4-131">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="bfda4-132">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-132">Medium</span></span>      |
| [<span data-ttu-id="bfda4-133">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="bfda4-133">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="bfda4-134">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-134">Medium</span></span>      |
| [<span data-ttu-id="bfda4-135">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="bfda4-135">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="bfda4-136">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-136">Medium</span></span>      |
| [<span data-ttu-id="bfda4-137">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="bfda4-137">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="bfda4-138">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-138">Medium</span></span>      |
| [<span data-ttu-id="bfda4-139">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="bfda4-139">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="bfda4-140">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-140">Medium</span></span>      |
| [<span data-ttu-id="bfda4-141">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="bfda4-141">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="bfda4-142">Средний</span><span class="sxs-lookup"><span data-stu-id="bfda4-142">Medium</span></span>      |
| [<span data-ttu-id="bfda4-143">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="bfda4-143">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="bfda4-144">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-144">Low</span></span>      |
| [<span data-ttu-id="bfda4-145">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="bfda4-145">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="bfda4-146">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-146">Low</span></span>      |
| [<span data-ttu-id="bfda4-147">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="bfda4-147">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="bfda4-148">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-148">Low</span></span>      |
| [<span data-ttu-id="bfda4-149">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="bfda4-149">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="bfda4-150">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-150">Low</span></span>      |
| [<span data-ttu-id="bfda4-151">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="bfda4-151">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="bfda4-152">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-152">Low</span></span>      |
| [<span data-ttu-id="bfda4-153">Собственные сущности нельзя запрашивать без использования запроса отслеживания владельцем</span><span class="sxs-lookup"><span data-stu-id="bfda4-153">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="bfda4-154">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-154">Low</span></span>      |
| [<span data-ttu-id="bfda4-155">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="bfda4-155">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="bfda4-156">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-156">Low</span></span>      |
| [<span data-ttu-id="bfda4-157">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="bfda4-157">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="bfda4-158">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-158">Low</span></span>      |
| [<span data-ttu-id="bfda4-159">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="bfda4-159">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="bfda4-160">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-160">Low</span></span>      |
| [<span data-ttu-id="bfda4-161">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="bfda4-161">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="bfda4-162">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-162">Low</span></span>      |
| [<span data-ttu-id="bfda4-163">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="bfda4-163">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="bfda4-164">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-164">Low</span></span>      |
| [<span data-ttu-id="bfda4-165">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="bfda4-165">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="bfda4-166">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-166">Low</span></span>      |
| [<span data-ttu-id="bfda4-167">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="bfda4-167">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="bfda4-168">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-168">Low</span></span>      |
| [<span data-ttu-id="bfda4-169">AddEntityFramework\* добавляет IMemoryCache с ограниченным размером</span><span class="sxs-lookup"><span data-stu-id="bfda4-169">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="bfda4-170">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-170">Low</span></span>      |
| [<span data-ttu-id="bfda4-171">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="bfda4-171">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="bfda4-172">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-172">Low</span></span>      |
| [<span data-ttu-id="bfda4-173">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bfda4-173">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="bfda4-174">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-174">Low</span></span>      |
| [<span data-ttu-id="bfda4-175">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="bfda4-175">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="bfda4-176">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-176">Low</span></span>      |
| [<span data-ttu-id="bfda4-177">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="bfda4-177">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="bfda4-178">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-178">Low</span></span>      |
| [<span data-ttu-id="bfda4-179">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bfda4-179">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="bfda4-180">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-180">Low</span></span>      |
| [<span data-ttu-id="bfda4-181">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="bfda4-181">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="bfda4-182">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-182">Low</span></span>      |
| [<span data-ttu-id="bfda4-183">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="bfda4-183">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="bfda4-184">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-184">Low</span></span>      |
| [<span data-ttu-id="bfda4-185">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bfda4-185">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="bfda4-186">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-186">Low</span></span>      |
| [<span data-ttu-id="bfda4-187">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="bfda4-187">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="bfda4-188">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-188">Low</span></span>      |
| [<span data-ttu-id="bfda4-189">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="bfda4-189">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="bfda4-190">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-190">Low</span></span>      |
| [<span data-ttu-id="bfda4-191">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bfda4-191">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="bfda4-192">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-192">Low</span></span>      |
| [<span data-ttu-id="bfda4-193">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="bfda4-193">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="bfda4-194">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-194">Low</span></span>      |
| [<span data-ttu-id="bfda4-195">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="bfda4-195">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="bfda4-196">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-196">Low</span></span>      |
| [<span data-ttu-id="bfda4-197">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="bfda4-197">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="bfda4-198">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-198">Low</span></span>      |
| [<span data-ttu-id="bfda4-199">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="bfda4-199">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="bfda4-200">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-200">Low</span></span>      |
| [<span data-ttu-id="bfda4-201">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="bfda4-201">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="bfda4-202">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-202">Low</span></span>      |
| [<span data-ttu-id="bfda4-203">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="bfda4-203">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="bfda4-204">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-204">Low</span></span>      |
| [<span data-ttu-id="bfda4-205">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="bfda4-205">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="bfda4-206">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-206">Low</span></span>      |
| [<span data-ttu-id="bfda4-207">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="bfda4-207">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="bfda4-208">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-208">Low</span></span>      |
| [<span data-ttu-id="bfda4-209">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bfda4-209">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="bfda4-210">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-210">Low</span></span>      |
| [<span data-ttu-id="bfda4-211">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bfda4-211">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="bfda4-212">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-212">Low</span></span>      |
| [<span data-ttu-id="bfda4-213">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="bfda4-213">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="bfda4-214">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-214">Low</span></span>      |
| [<span data-ttu-id="bfda4-215">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="bfda4-215">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="bfda4-216">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-216">Low</span></span>      |
| [<span data-ttu-id="bfda4-217">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="bfda4-217">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="bfda4-218">Низкий</span><span class="sxs-lookup"><span data-stu-id="bfda4-218">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="bfda4-219">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="bfda4-219">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="bfda4-220">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="bfda4-220">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="bfda4-221">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-221">**Old behavior**</span></span>

<span data-ttu-id="bfda4-222">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="bfda4-222">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="bfda4-223">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-223">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="bfda4-224">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-224">**New behavior**</span></span>

<span data-ttu-id="bfda4-225">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="bfda4-225">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="bfda4-226">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-226">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="bfda4-227">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-227">**Why**</span></span>

<span data-ttu-id="bfda4-228">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="bfda4-228">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="bfda4-229">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="bfda4-229">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="bfda4-230">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="bfda4-230">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="bfda4-231">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="bfda4-231">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="bfda4-232">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="bfda4-232">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="bfda4-233">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="bfda4-233">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="bfda4-234">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-234">**Mitigations**</span></span>

<span data-ttu-id="bfda4-235">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="bfda4-235">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="bfda4-236">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="bfda4-236">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="bfda4-237">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="bfda4-237">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="bfda4-238">EF Core 3.1 снова ориентируется на .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="bfda4-238">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="bfda4-239">В связи с этим поддержка .NET Framework восстановлена.</span><span class="sxs-lookup"><span data-stu-id="bfda4-239">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="bfda4-240">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-240">**Old behavior**</span></span>

<span data-ttu-id="bfda4-241">До версии 3.0 EF Core был предназначен для .NET Standard 2.0 и выполнялся на всех платформах, поддерживающих этот стандарт, включая .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bfda4-241">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="bfda4-242">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-242">**New behavior**</span></span>

<span data-ttu-id="bfda4-243">Начиная с версии 3.0, EF Core предназначен для .NET Standard 2.1 и выполняется на всех платформах, поддерживающих этот стандарт.</span><span class="sxs-lookup"><span data-stu-id="bfda4-243">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="bfda4-244">Сюда не входит .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bfda4-244">This does not include .NET Framework.</span></span>

<span data-ttu-id="bfda4-245">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-245">**Why**</span></span>

<span data-ttu-id="bfda4-246">Это часть стратегического решения по технологиям .NET, направленного на работу над .NET Core и другими современными платформами .NET, такими как Xamarin.</span><span class="sxs-lookup"><span data-stu-id="bfda4-246">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="bfda4-247">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-247">**Mitigations**</span></span>

<span data-ttu-id="bfda4-248">Используйте EF Core 3.1.</span><span class="sxs-lookup"><span data-stu-id="bfda4-248">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="bfda4-249">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfda4-249">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="bfda4-250">Отслеживание объявлений о вопросе № 325</span><span class="sxs-lookup"><span data-stu-id="bfda4-250">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="bfda4-251">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-251">**Old behavior**</span></span>

<span data-ttu-id="bfda4-252">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bfda4-252">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="bfda4-253">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-253">**New behavior**</span></span>

<span data-ttu-id="bfda4-254">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-254">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="bfda4-255">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-255">**Why**</span></span>

<span data-ttu-id="bfda4-256">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bfda4-256">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="bfda4-257">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="bfda4-257">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="bfda4-258">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-258">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="bfda4-259">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-259">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="bfda4-260">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-260">**Mitigations**</span></span>

<span data-ttu-id="bfda4-261">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="bfda4-261">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="bfda4-262">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="bfda4-262">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="bfda4-263">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="bfda4-263">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="bfda4-264">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-264">**Old behavior**</span></span>

<span data-ttu-id="bfda4-265">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="bfda4-265">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="bfda4-266">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-266">**New behavior**</span></span>

<span data-ttu-id="bfda4-267">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="bfda4-267">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="bfda4-268">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-268">**Why**</span></span>

<span data-ttu-id="bfda4-269">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="bfda4-269">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="bfda4-270">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-270">**Mitigations**</span></span>

<span data-ttu-id="bfda4-271">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="bfda4-271">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="bfda4-272">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](/dotnet/core/tools/global-tools#install-a-local-tool).</span><span class="sxs-lookup"><span data-stu-id="bfda4-272">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](/dotnet/core/tools/global-tools#install-a-local-tool).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="bfda4-273">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="bfda4-273">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="bfda4-274">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="bfda4-274">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="bfda4-275">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-275">**Old behavior**</span></span>

<span data-ttu-id="bfda4-276">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="bfda4-276">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="bfda4-277">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-277">**New behavior**</span></span>

<span data-ttu-id="bfda4-278">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="bfda4-278">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="bfda4-279">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-279">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="bfda4-280">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="bfda4-280">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="bfda4-281">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-281">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="bfda4-282">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-282">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="bfda4-283">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-283">**Why**</span></span>

<span data-ttu-id="bfda4-284">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="bfda4-284">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="bfda4-285">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="bfda4-285">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="bfda4-286">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-286">**Mitigations**</span></span>

<span data-ttu-id="bfda4-287">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-287">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="bfda4-288">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="bfda4-288">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="bfda4-289">Отслеживание вопроса № 15392</span><span class="sxs-lookup"><span data-stu-id="bfda4-289">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="bfda4-290">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-290">**Old behavior**</span></span>

<span data-ttu-id="bfda4-291">До версии EF Core 3.0 метод FromSql пытался определить, можно ли выполнить составление для переданного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="bfda4-291">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="bfda4-292">Если SQL-запрос не допускал составления, как в случае с хранимой процедурой, вычисление производилось на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="bfda4-292">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="bfda4-293">В случае с приведенным ниже запросом хранимая процедура выполнялась на сервере, а метод FirstOrDefault — на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="bfda4-293">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="bfda4-294">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-294">**New behavior**</span></span>

<span data-ttu-id="bfda4-295">Начиная с версии 3.0 платформа EF Core не пытается анализировать код SQL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-295">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="bfda4-296">Поэтому если вы выполняете составление после метода FromSqlRaw или FromSqlInterpolated, EF Core составляет SQL-запрос, вызывая вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="bfda4-296">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="bfda4-297">При использовании хранимой процедуры с составлением вы получите исключение, вызванное недопустимым синтаксисом SQL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-297">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="bfda4-298">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-298">**Why**</span></span>

<span data-ttu-id="bfda4-299">EF Core 3.0 не поддерживает автоматическое вычисление на стороне клиента, так как оно было подвержено ошибкам, как описано [здесь](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="bfda4-299">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="bfda4-300">**Меры по снижению риска**</span><span class="sxs-lookup"><span data-stu-id="bfda4-300">**Mitigation**</span></span>

<span data-ttu-id="bfda4-301">Если вы используете хранимую процедуру в методе FromSqlRaw или FromSqlInterpolated, то знаете, что составление для нее невозможно, поэтому вы можете добавить __AsEnumerable или AsAsyncEnumerable__ сразу после вызова метода FromSql, чтобы избежать составления на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="bfda4-301">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="bfda4-302">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="bfda4-302">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="bfda4-303">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="bfda4-303">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="bfda4-304">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-304">**Old behavior**</span></span>

<span data-ttu-id="bfda4-305">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="bfda4-305">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="bfda4-306">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-306">**New behavior**</span></span>

<span data-ttu-id="bfda4-307">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-307">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="bfda4-308">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="bfda4-308">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="bfda4-309">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-309">**Why**</span></span>

<span data-ttu-id="bfda4-310">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="bfda4-310">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="bfda4-311">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-311">**Mitigations**</span></span>

<span data-ttu-id="bfda4-312">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="bfda4-312">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="bfda4-313">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="bfda4-313">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="bfda4-314">Отслеживание вопроса № 13518</span><span class="sxs-lookup"><span data-stu-id="bfda4-314">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="bfda4-315">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-315">**Old behavior**</span></span>

<span data-ttu-id="bfda4-316">До EF Core 3.0 один и тот же экземпляр сущности будет использоваться для каждого вхождения сущности с заданным типом и ИД.</span><span class="sxs-lookup"><span data-stu-id="bfda4-316">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="bfda4-317">Это соответствует поведению запросов отслеживания.</span><span class="sxs-lookup"><span data-stu-id="bfda4-317">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="bfda4-318">Например, следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="bfda4-318">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="bfda4-319">возвращает тот же экземпляр `Category` для каждого `Product`, связанного с заданной категорией.</span><span class="sxs-lookup"><span data-stu-id="bfda4-319">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="bfda4-320">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-320">**New behavior**</span></span>

<span data-ttu-id="bfda4-321">Начиная с EF Core 3.0, при обнаружении сущности с заданным типом и ИД в разных местах возвращенного графа будут создаваться различные экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-321">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="bfda4-322">Например, вышеприведенный запрос теперь будет возвращать новый экземпляр `Category` для каждого `Product`, даже если два продукта связаны с одной категорией.</span><span class="sxs-lookup"><span data-stu-id="bfda4-322">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="bfda4-323">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-323">**Why**</span></span>

<span data-ttu-id="bfda4-324">Разрешение идентификаторов (то есть определение того, что сущность имеет тот же тип и идентификатор, что и обнаруженная ранее сущность) добавляет дополнительную нагрузку на производительность и память.</span><span class="sxs-lookup"><span data-stu-id="bfda4-324">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="bfda4-325">Обычно это идет вразрез с причинами использования запросов без отслеживания.</span><span class="sxs-lookup"><span data-stu-id="bfda4-325">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="bfda4-326">Кроме того, хотя разрешение идентификаторов иногда может быть полезным, оно не требуется, если сущности должны быть сериализованы и отправлены клиенту, что является распространенным случаем для неотслеживаемых запросов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-326">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="bfda4-327">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-327">**Mitigations**</span></span>

<span data-ttu-id="bfda4-328">Используйте запрос с отслеживанием, если требуется разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-328">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="bfda4-329">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="bfda4-329">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="bfda4-330">Отслеживание вопроса № 14523</span><span class="sxs-lookup"><span data-stu-id="bfda4-330">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="bfda4-331">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="bfda4-331">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="bfda4-332">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="bfda4-332">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="bfda4-333">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="bfda4-333">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="bfda4-334">Отслеживание вопроса № 12378</span><span class="sxs-lookup"><span data-stu-id="bfda4-334">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="bfda4-335">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-335">**Old behavior**</span></span>

<span data-ttu-id="bfda4-336">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="bfda4-336">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="bfda4-337">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="bfda4-337">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="bfda4-338">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-338">**New behavior**</span></span>

<span data-ttu-id="bfda4-339">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-339">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="bfda4-340">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-340">**Why**</span></span>

<span data-ttu-id="bfda4-341">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-341">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="bfda4-342">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-342">**Mitigations**</span></span>

<span data-ttu-id="bfda4-343">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-343">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="bfda4-344">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bfda4-344">This can be avoided by:</span></span>
* <span data-ttu-id="bfda4-345">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="bfda4-345">Not using store-generated keys.</span></span>
* <span data-ttu-id="bfda4-346">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="bfda4-346">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="bfda4-347">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-347">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="bfda4-348">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="bfda4-348">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="bfda4-349">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="bfda4-349">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="bfda4-350">Отслеживание вопроса № 14616</span><span class="sxs-lookup"><span data-stu-id="bfda4-350">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="bfda4-351">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-351">**Old behavior**</span></span>

<span data-ttu-id="bfda4-352">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-352">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="bfda4-353">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-353">**New behavior**</span></span>

<span data-ttu-id="bfda4-354">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-354">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="bfda4-355">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-355">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="bfda4-356">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="bfda4-356">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="bfda4-357">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-357">**Why**</span></span>

<span data-ttu-id="bfda4-358">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="bfda4-358">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="bfda4-359">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-359">**Mitigations**</span></span>

<span data-ttu-id="bfda4-360">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="bfda4-360">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="bfda4-361">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-361">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="bfda4-362">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="bfda4-362">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="bfda4-363">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="bfda4-363">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="bfda4-364">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="bfda4-364">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="bfda4-365">Отслеживание вопроса № 10114</span><span class="sxs-lookup"><span data-stu-id="bfda4-365">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="bfda4-366">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-366">**Old behavior**</span></span>

<span data-ttu-id="bfda4-367">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="bfda4-367">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="bfda4-368">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-368">**New behavior**</span></span>

<span data-ttu-id="bfda4-369">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="bfda4-369">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="bfda4-370">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-370">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="bfda4-371">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-371">**Why**</span></span>

<span data-ttu-id="bfda4-372">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед вызовом_  `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-372">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="bfda4-373">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-373">**Mitigations**</span></span>

<span data-ttu-id="bfda4-374">Для восстановления прежнего поведения можно использовать параметры `context.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-374">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="bfda4-375">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-375">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="bfda4-376">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="bfda4-376">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="bfda4-377">Отслеживание вопроса № 18022</span><span class="sxs-lookup"><span data-stu-id="bfda4-377">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="bfda4-378">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-378">**Old behavior**</span></span>

<span data-ttu-id="bfda4-379">До версии 3.0 безотложная загрузка навигаций по коллекциям с помощью операторов `Include` приводила к созданию нескольких запросов к реляционной базе данных, по одной для каждого связанного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-379">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="bfda4-380">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-380">**New behavior**</span></span>

<span data-ttu-id="bfda4-381">Начиная с версии 3.0 платформа EF Core создает один запрос с соединениями к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="bfda4-381">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="bfda4-382">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-382">**Why**</span></span>

<span data-ttu-id="bfda4-383">Отправка нескольких запросов для реализации одного запроса LINQ приводила ко множеству проблем, включая снижение производительности из-за необходимости нескольких циклов обращения к базе данных и проблем с согласованностью данных, так как при каждом запросе состояние базы данных могло быть разным.</span><span class="sxs-lookup"><span data-stu-id="bfda4-383">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="bfda4-384">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-384">**Mitigations**</span></span>

<span data-ttu-id="bfda4-385">Хотя технически это изменение не является критическим, оно может существенно повлиять на производительность приложения в случае, если один запрос содержит большое число операторов `Include` с навигациями по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="bfda4-385">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="bfda4-386">Дополнительные сведения и указания по повышению эффективности запросов см. в [этом комментарии](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085).</span><span class="sxs-lookup"><span data-stu-id="bfda4-386">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="bfda4-387">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="bfda4-387">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="bfda4-388">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="bfda4-388">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="bfda4-389">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-389">**Old behavior**</span></span>

<span data-ttu-id="bfda4-390">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="bfda4-390">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="bfda4-391">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-391">**New behavior**</span></span>

<span data-ttu-id="bfda4-392">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="bfda4-392">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="bfda4-393">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-393">**Why**</span></span>

<span data-ttu-id="bfda4-394">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-394">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="bfda4-395">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-395">**Mitigations**</span></span>

<span data-ttu-id="bfda4-396">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-396">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="bfda4-397">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="bfda4-397">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="bfda4-398">Отслеживание вопроса № 14194</span><span class="sxs-lookup"><span data-stu-id="bfda4-398">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="bfda4-399">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-399">**Old behavior**</span></span>

<span data-ttu-id="bfda4-400">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/keyless-entity-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="bfda4-400">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="bfda4-401">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="bfda4-401">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="bfda4-402">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-402">**New behavior**</span></span>

<span data-ttu-id="bfda4-403">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="bfda4-403">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="bfda4-404">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="bfda4-404">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="bfda4-405">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-405">**Why**</span></span>

<span data-ttu-id="bfda4-406">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-406">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="bfda4-407">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-407">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="bfda4-408">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="bfda4-408">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="bfda4-409">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-409">**Mitigations**</span></span>

<span data-ttu-id="bfda4-410">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="bfda4-410">The following parts of the API are now obsolete:</span></span>

* <span data-ttu-id="bfda4-411">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="bfda4-411">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="bfda4-412">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="bfda4-412">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="bfda4-413">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-413">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="bfda4-414">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-414">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>
* <span data-ttu-id="bfda4-415">**`IQueryTypeConfiguration<TQuery>`**  — вместо него следует использовать `IEntityTypeConfiguration<TEntity>`\*\*.</span><span class="sxs-lookup"><span data-stu-id="bfda4-415">**`IQueryTypeConfiguration<TQuery>`** - Instead `IEntityTypeConfiguration<TEntity>`\*\* should be used.</span></span>

> [!NOTE]
> <span data-ttu-id="bfda4-416">Из-за [проблемы в версиях 3.x](https://github.com/dotnet/efcore/issues/19537) при запросе сущностей без ключей, все свойства которых имеют значение `null`, возвращается `null` вместо сущности. Если эта проблема затрагивает ваш сценарий, также добавьте логику для обработки `null` в результатах.</span><span class="sxs-lookup"><span data-stu-id="bfda4-416">Due to [an issue in 3.x](https://github.com/dotnet/efcore/issues/19537) when querying keyless entities that have all properties set to `null` a `null` will be returned instead of an entity, if this issue is applicable to your scenario also add logic to handle `null` in results.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="bfda4-417">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="bfda4-417">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="bfda4-418">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="bfda4-418">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="bfda4-419">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-419">**Old behavior**</span></span>

<span data-ttu-id="bfda4-420">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-420">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="bfda4-421">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-421">**New behavior**</span></span>

<span data-ttu-id="bfda4-422">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-422">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="bfda4-423">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-423">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="bfda4-424">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-424">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="bfda4-425">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-425">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="bfda4-426">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-426">For example:</span></span>

```csharp
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

<span data-ttu-id="bfda4-427">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-427">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="bfda4-428">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-428">**Why**</span></span>

<span data-ttu-id="bfda4-429">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="bfda4-429">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="bfda4-430">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-430">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="bfda4-431">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-431">**Mitigations**</span></span>

<span data-ttu-id="bfda4-432">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="bfda4-432">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="bfda4-433">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="bfda4-433">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="bfda4-434">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="bfda4-434">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="bfda4-435">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-435">**Old behavior**</span></span>

<span data-ttu-id="bfda4-436">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="bfda4-436">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="bfda4-437">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-437">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="bfda4-438">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-438">**New behavior**</span></span>

<span data-ttu-id="bfda4-439">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-439">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="bfda4-440">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-440">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="bfda4-441">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-441">**Mitigations**</span></span>

<span data-ttu-id="bfda4-442">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-442">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="bfda4-443">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-443">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="bfda4-444">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="bfda4-444">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="bfda4-445">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="bfda4-445">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="bfda4-446">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-446">**Old behavior**</span></span>

<span data-ttu-id="bfda4-447">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="bfda4-447">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="bfda4-448">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="bfda4-448">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="bfda4-449">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-449">**New behavior**</span></span>

<span data-ttu-id="bfda4-450">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-450">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="bfda4-451">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-451">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="bfda4-452">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-452">**Why**</span></span>

<span data-ttu-id="bfda4-453">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="bfda4-453">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bfda4-454">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-454">**Mitigations**</span></span>

<span data-ttu-id="bfda4-455">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="bfda4-455">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="bfda4-456">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="bfda4-456">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="bfda4-457">Собственные сущности нельзя запрашивать без использования запроса отслеживания владельцем</span><span class="sxs-lookup"><span data-stu-id="bfda4-457">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="bfda4-458">Отслеживание вопроса № 18876</span><span class="sxs-lookup"><span data-stu-id="bfda4-458">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="bfda4-459">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-459">**Old behavior**</span></span>

<span data-ttu-id="bfda4-460">До версии EF Core 3.0 собственные сущности можно было запрашивать как любую другую навигацию.</span><span class="sxs-lookup"><span data-stu-id="bfda4-460">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="bfda4-461">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-461">**New behavior**</span></span>

<span data-ttu-id="bfda4-462">Начиная с версии 3.0 EF Core выдает исключение, если запрос отслеживания проецирует собственную сущность без владельца.</span><span class="sxs-lookup"><span data-stu-id="bfda4-462">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="bfda4-463">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-463">**Why**</span></span>

<span data-ttu-id="bfda4-464">Выполнять операции с собственными сущностями без владельца невозможно, поэтому в подавляющем большинстве случаев подобные запросы к ним являются ошибкой.</span><span class="sxs-lookup"><span data-stu-id="bfda4-464">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="bfda4-465">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-465">**Mitigations**</span></span>

<span data-ttu-id="bfda4-466">Если собственную сущность следует отслеживать для изменения в последующем, владелец должен быть включен в запрос.</span><span class="sxs-lookup"><span data-stu-id="bfda4-466">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="bfda4-467">В противном случае добавьте вызов `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="bfda4-467">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="bfda4-468">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="bfda4-468">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="bfda4-469">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="bfda4-469">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="bfda4-470">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-470">**Old behavior**</span></span>

<span data-ttu-id="bfda4-471">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="bfda4-471">Consider the following model:</span></span>
```csharp
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

<span data-ttu-id="bfda4-472">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bfda4-472">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="bfda4-473">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-473">**New behavior**</span></span>

<span data-ttu-id="bfda4-474">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-474">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="bfda4-475">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-475">**Why**</span></span>

<span data-ttu-id="bfda4-476">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="bfda4-476">The old behavoir was unexpected.</span></span>

<span data-ttu-id="bfda4-477">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-477">**Mitigations**</span></span>

<span data-ttu-id="bfda4-478">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-478">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="bfda4-479">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="bfda4-479">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="bfda4-480">Отслеживание вопроса № 13274</span><span class="sxs-lookup"><span data-stu-id="bfda4-480">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="bfda4-481">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-481">**Old behavior**</span></span>

<span data-ttu-id="bfda4-482">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="bfda4-482">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="bfda4-483">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="bfda4-483">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="bfda4-484">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="bfda4-484">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="bfda4-485">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-485">**New behavior**</span></span>

<span data-ttu-id="bfda4-486">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="bfda4-486">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="bfda4-487">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="bfda4-487">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="bfda4-488">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-488">For example:</span></span>

```csharp
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

```csharp
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

<span data-ttu-id="bfda4-489">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-489">**Why**</span></span>

<span data-ttu-id="bfda4-490">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="bfda4-490">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="bfda4-491">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-491">**Mitigations**</span></span>

<span data-ttu-id="bfda4-492">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="bfda4-492">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="bfda4-493">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="bfda4-493">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="bfda4-494">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="bfda4-494">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="bfda4-495">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-495">**Old behavior**</span></span>

<span data-ttu-id="bfda4-496">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-496">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
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

<span data-ttu-id="bfda4-497">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-497">**New behavior**</span></span>

<span data-ttu-id="bfda4-498">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="bfda4-498">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="bfda4-499">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-499">**Why**</span></span>

<span data-ttu-id="bfda4-500">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-500">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="bfda4-501">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="bfda4-501">The new behavior also matches EF6.</span></span>

<span data-ttu-id="bfda4-502">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-502">**Mitigations**</span></span>

<span data-ttu-id="bfda4-503">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="bfda4-503">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="bfda4-504">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="bfda4-504">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="bfda4-505">Отслеживание вопроса № 6872</span><span class="sxs-lookup"><span data-stu-id="bfda4-505">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="bfda4-506">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-506">**Old behavior**</span></span>

<span data-ttu-id="bfda4-507">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-507">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="bfda4-508">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-508">**New behavior**</span></span>

<span data-ttu-id="bfda4-509">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-509">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="bfda4-510">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="bfda4-510">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="bfda4-511">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-511">**Why**</span></span>

<span data-ttu-id="bfda4-512">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="bfda4-512">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="bfda4-513">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-513">**Mitigations**</span></span>

<span data-ttu-id="bfda4-514">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="bfda4-514">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="bfda4-515">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-515">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="bfda4-516">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="bfda4-516">Backing fields are used by default</span></span>

[<span data-ttu-id="bfda4-517">Отслеживание вопроса № 12430</span><span class="sxs-lookup"><span data-stu-id="bfda4-517">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="bfda4-518">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-518">**Old behavior**</span></span>

<span data-ttu-id="bfda4-519">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="bfda4-519">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="bfda4-520">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="bfda4-520">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="bfda4-521">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-521">**New behavior**</span></span>

<span data-ttu-id="bfda4-522">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="bfda4-522">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="bfda4-523">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="bfda4-523">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="bfda4-524">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-524">**Why**</span></span>

<span data-ttu-id="bfda4-525">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-525">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="bfda4-526">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-526">**Mitigations**</span></span>

<span data-ttu-id="bfda4-527">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-527">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="bfda4-528">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-528">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="bfda4-529">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="bfda4-529">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="bfda4-530">Отслеживание вопроса № 12523</span><span class="sxs-lookup"><span data-stu-id="bfda4-530">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="bfda4-531">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-531">**Old behavior**</span></span>

<span data-ttu-id="bfda4-532">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-532">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="bfda4-533">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="bfda4-533">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="bfda4-534">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-534">**New behavior**</span></span>

<span data-ttu-id="bfda4-535">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-535">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="bfda4-536">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-536">**Why**</span></span>

<span data-ttu-id="bfda4-537">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="bfda4-537">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="bfda4-538">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-538">**Mitigations**</span></span>

<span data-ttu-id="bfda4-539">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="bfda4-539">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="bfda4-540">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="bfda4-540">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="bfda4-541">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="bfda4-541">Field-only property names should match the field name</span></span>

<span data-ttu-id="bfda4-542">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-542">**Old behavior**</span></span>

<span data-ttu-id="bfda4-543">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе .NET, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-543">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="bfda4-544">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-544">**New behavior**</span></span>

<span data-ttu-id="bfda4-545">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="bfda4-545">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="bfda4-546">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-546">**Why**</span></span>

<span data-ttu-id="bfda4-547">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="bfda4-547">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="bfda4-548">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-548">**Mitigations**</span></span>

<span data-ttu-id="bfda4-549">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="bfda4-549">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="bfda4-550">В будущем выпуске EF Core после версии 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства (см. вопрос [№ 15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="bfda4-550">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="bfda4-551">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="bfda4-551">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="bfda4-552">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="bfda4-552">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="bfda4-553">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-553">**Old behavior**</span></span>

<span data-ttu-id="bfda4-554">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов методов [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bfda4-554">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="bfda4-555">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-555">**New behavior**</span></span>

<span data-ttu-id="bfda4-556">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="bfda4-556">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="bfda4-557">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-557">**Why**</span></span>

<span data-ttu-id="bfda4-558">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-558">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="bfda4-559">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-559">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="bfda4-560">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-560">**Mitigations**</span></span>

<span data-ttu-id="bfda4-561">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bfda4-561">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="bfda4-562">AddEntityFramework\* добавляет IMemoryCache с ограниченным размером</span><span class="sxs-lookup"><span data-stu-id="bfda4-562">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="bfda4-563">Отслеживание вопроса № 12905</span><span class="sxs-lookup"><span data-stu-id="bfda4-563">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="bfda4-564">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-564">**Old behavior**</span></span>

<span data-ttu-id="bfda4-565">До EF Core 3.0 вызов методов `AddEntityFramework*` также приводил к регистрации служб кэширования памяти с внедрением зависимостей без ограничения размера.</span><span class="sxs-lookup"><span data-stu-id="bfda4-565">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="bfda4-566">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-566">**New behavior**</span></span>

<span data-ttu-id="bfda4-567">Начиная с версии EF Core 3.0 вызов `AddEntityFramework*` приводит к регистрации службы IMemoryCache с ограничением размера.</span><span class="sxs-lookup"><span data-stu-id="bfda4-567">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="bfda4-568">Если добавленные позже службы зависят от IMemoryCache, они могут быстро достичь ограничения по умолчанию, что приведет к созданию исключений или снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-568">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="bfda4-569">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-569">**Why**</span></span>

<span data-ttu-id="bfda4-570">Использование IMemoryCache без ограничения может привести к неконтролируемому использованию памяти, если в логике кэширования запросов есть ошибка или запросы создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="bfda4-570">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="bfda4-571">Ограничение по умолчанию позволяет предотвращать возможные атаки типа "отказ в обслуживании".</span><span class="sxs-lookup"><span data-stu-id="bfda4-571">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="bfda4-572">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-572">**Mitigations**</span></span>

<span data-ttu-id="bfda4-573">В большинстве случаев вызывать `AddEntityFramework*` не требуется, если также вызывается метод `AddDbContext` или `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-573">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="bfda4-574">Поэтому лучшее решение — удалить вызов `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-574">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="bfda4-575">Если для работы вашего приложения нужны эти службы, заранее зарегистрируйте реализацию IMemoryCache явным образом в контейнере внедрения зависимостей с помощью метода [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bfda4-575">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="bfda4-576">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="bfda4-576">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="bfda4-577">Отслеживание вопроса № 13552</span><span class="sxs-lookup"><span data-stu-id="bfda4-577">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="bfda4-578">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-578">**Old behavior**</span></span>

<span data-ttu-id="bfda4-579">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="bfda4-579">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="bfda4-580">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="bfda4-580">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="bfda4-581">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-581">**New behavior**</span></span>

<span data-ttu-id="bfda4-582">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-582">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="bfda4-583">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-583">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="bfda4-584">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-584">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="bfda4-585">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="bfda4-585">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="bfda4-586">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-586">**Why**</span></span>

<span data-ttu-id="bfda4-587">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-587">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="bfda4-588">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-588">**Mitigations**</span></span>

<span data-ttu-id="bfda4-589">Вызовите `ChangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="bfda4-589">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="bfda4-590">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bfda4-590">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="bfda4-591">Отслеживание вопроса № 14617</span><span class="sxs-lookup"><span data-stu-id="bfda4-591">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="bfda4-592">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-592">**Old behavior**</span></span>

<span data-ttu-id="bfda4-593">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-593">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="bfda4-594">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-594">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="bfda4-595">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-595">**New behavior**</span></span>

<span data-ttu-id="bfda4-596">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="bfda4-596">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="bfda4-597">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-597">**Why**</span></span>

<span data-ttu-id="bfda4-598">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="bfda4-598">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="bfda4-599">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-599">**Mitigations**</span></span>

<span data-ttu-id="bfda4-600">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-600">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="bfda4-601">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="bfda4-601">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="bfda4-602">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="bfda4-602">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="bfda4-603">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="bfda4-603">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="bfda4-604">Отслеживание вопроса № 14698</span><span class="sxs-lookup"><span data-stu-id="bfda4-604">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="bfda4-605">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-605">**Old behavior**</span></span>

<span data-ttu-id="bfda4-606">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="bfda4-606">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="bfda4-607">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-607">**New behavior**</span></span>

<span data-ttu-id="bfda4-608">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="bfda4-608">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="bfda4-609">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-609">**Why**</span></span>

<span data-ttu-id="bfda4-610">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="bfda4-610">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="bfda4-611">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-611">**Mitigations**</span></span>

<span data-ttu-id="bfda4-612">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-612">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="bfda4-613">Это нетипично.</span><span class="sxs-lookup"><span data-stu-id="bfda4-613">This isn't common.</span></span>
<span data-ttu-id="bfda4-614">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="bfda4-614">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="bfda4-615">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="bfda4-615">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="bfda4-616">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="bfda4-616">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="bfda4-617">Отслеживание вопроса № 12780</span><span class="sxs-lookup"><span data-stu-id="bfda4-617">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="bfda4-618">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-618">**Old behavior**</span></span>

<span data-ttu-id="bfda4-619">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="bfda4-619">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="bfda4-620">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="bfda4-620">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="bfda4-621">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="bfda4-621">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="bfda4-622">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-622">**New behavior**</span></span>

<span data-ttu-id="bfda4-623">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="bfda4-623">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="bfda4-624">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-624">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="bfda4-625">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="bfda4-625">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="bfda4-626">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-626">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="bfda4-627">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-627">**Why**</span></span>

<span data-ttu-id="bfda4-628">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-628">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="bfda4-629">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-629">**Mitigations**</span></span>

<span data-ttu-id="bfda4-630">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="bfda4-630">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="bfda4-631">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bfda4-631">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="bfda4-632">Отслеживание вопроса № 10236</span><span class="sxs-lookup"><span data-stu-id="bfda4-632">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="bfda4-633">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-633">**Old behavior**</span></span>

<span data-ttu-id="bfda4-634">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-634">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="bfda4-635">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-635">**New behavior**</span></span>

<span data-ttu-id="bfda4-636">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="bfda4-636">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="bfda4-637">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-637">**Why**</span></span>

<span data-ttu-id="bfda4-638">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="bfda4-638">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="bfda4-639">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-639">**Mitigations**</span></span>

<span data-ttu-id="bfda4-640">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="bfda4-640">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="bfda4-641">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-641">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="bfda4-642">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-642">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="bfda4-643">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="bfda4-643">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="bfda4-644">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="bfda4-644">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="bfda4-645">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-645">**Old behavior**</span></span>

<span data-ttu-id="bfda4-646">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="bfda4-646">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="bfda4-647">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-647">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="bfda4-648">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="bfda4-648">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="bfda4-649">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="bfda4-649">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="bfda4-650">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-650">**New behavior**</span></span>

<span data-ttu-id="bfda4-651">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="bfda4-651">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="bfda4-652">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-652">**Why**</span></span>

<span data-ttu-id="bfda4-653">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="bfda4-653">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="bfda4-654">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-654">**Mitigations**</span></span>

<span data-ttu-id="bfda4-655">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="bfda4-655">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="bfda4-656">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="bfda4-656">This is not common.</span></span>
<span data-ttu-id="bfda4-657">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="bfda4-657">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="bfda4-658">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-658">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="bfda4-659">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="bfda4-659">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="bfda4-660">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="bfda4-660">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="bfda4-661">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-661">**Old behavior**</span></span>

<span data-ttu-id="bfda4-662">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="bfda4-662">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="bfda4-663">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="bfda4-663">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="bfda4-664">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-664">**New behavior**</span></span>

<span data-ttu-id="bfda4-665">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="bfda4-665">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="bfda4-666">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-666">**Why**</span></span>

<span data-ttu-id="bfda4-667">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="bfda4-667">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="bfda4-668">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-668">**Mitigations**</span></span>

<span data-ttu-id="bfda4-669">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="bfda4-669">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="bfda4-670">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-670">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="bfda4-671">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-671">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="bfda4-672">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bfda4-672">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="bfda4-673">Отслеживание вопроса № 9913</span><span class="sxs-lookup"><span data-stu-id="bfda4-673">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="bfda4-674">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-674">**Old behavior**</span></span>

<span data-ttu-id="bfda4-675">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bfda4-675">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="bfda4-676">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-676">**New behavior**</span></span>

<span data-ttu-id="bfda4-677">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bfda4-677">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="bfda4-678">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-678">**Why**</span></span>

<span data-ttu-id="bfda4-679">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="bfda4-679">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="bfda4-680">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-680">**Mitigations**</span></span>

<span data-ttu-id="bfda4-681">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="bfda4-681">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="bfda4-682">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="bfda4-682">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="bfda4-683">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="bfda4-683">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="bfda4-684">Отслеживание вопроса № 11811</span><span class="sxs-lookup"><span data-stu-id="bfda4-684">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="bfda4-685">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-685">**Old behavior**</span></span>

<span data-ttu-id="bfda4-686">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="bfda4-686">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="bfda4-687">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-687">**New behavior**</span></span>

<span data-ttu-id="bfda4-688">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="bfda4-688">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="bfda4-689">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-689">**Why**</span></span>

<span data-ttu-id="bfda4-690">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="bfda4-690">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="bfda4-691">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="bfda4-691">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="bfda4-692">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-692">**Mitigations**</span></span>

<span data-ttu-id="bfda4-693">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="bfda4-693">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="bfda4-694">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="bfda4-694">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="bfda4-695">Отслеживание вопроса № 12366</span><span class="sxs-lookup"><span data-stu-id="bfda4-695">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="bfda4-696">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-696">**Old behavior**</span></span>

<span data-ttu-id="bfda4-697">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-697">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="bfda4-698">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-698">**New behavior**</span></span>

<span data-ttu-id="bfda4-699">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="bfda4-699">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="bfda4-700">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-700">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="bfda4-701">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-701">**Why**</span></span>

<span data-ttu-id="bfda4-702">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="bfda4-702">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="bfda4-703">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-703">**Mitigations**</span></span>

<span data-ttu-id="bfda4-704">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="bfda4-704">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="bfda4-705">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="bfda4-705">Metadata API changes</span></span>

[<span data-ttu-id="bfda4-706">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="bfda4-706">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bfda4-707">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-707">**New behavior**</span></span>

<span data-ttu-id="bfda4-708">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="bfda4-708">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="bfda4-709">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-709">**Why**</span></span>

<span data-ttu-id="bfda4-710">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-710">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="bfda4-711">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-711">**Mitigations**</span></span>

<span data-ttu-id="bfda4-712">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-712">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="bfda4-713">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="bfda4-713">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="bfda4-714">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="bfda4-714">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bfda4-715">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-715">**New behavior**</span></span>

<span data-ttu-id="bfda4-716">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="bfda4-716">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="bfda4-717">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-717">**Why**</span></span>

<span data-ttu-id="bfda4-718">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-718">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="bfda4-719">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-719">**Mitigations**</span></span>

<span data-ttu-id="bfda4-720">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-720">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="bfda4-721">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="bfda4-721">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="bfda4-722">Отслеживание вопроса № 12151</span><span class="sxs-lookup"><span data-stu-id="bfda4-722">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="bfda4-723">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-723">**Old behavior**</span></span>

<span data-ttu-id="bfda4-724">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="bfda4-724">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bfda4-725">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-725">**New behavior**</span></span>

<span data-ttu-id="bfda4-726">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="bfda4-726">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bfda4-727">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-727">**Why**</span></span>

<span data-ttu-id="bfda4-728">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-728">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="bfda4-729">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-729">**Mitigations**</span></span>

<span data-ttu-id="bfda4-730">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-730">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="bfda4-731">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-731">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="bfda4-732">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bfda4-732">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="bfda4-733">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-733">**Old behavior**</span></span>

<span data-ttu-id="bfda4-734">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-734">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="bfda4-735">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-735">**New behavior**</span></span>

<span data-ttu-id="bfda4-736">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-736">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="bfda4-737">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-737">**Why**</span></span>

<span data-ttu-id="bfda4-738">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="bfda4-738">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="bfda4-739">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-739">**Mitigations**</span></span>

<span data-ttu-id="bfda4-740">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-740">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bfda4-741">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="bfda4-741">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bfda4-742">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="bfda4-742">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="bfda4-743">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-743">**Old behavior**</span></span>

<span data-ttu-id="bfda4-744">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-744">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="bfda4-745">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-745">**New behavior**</span></span>

<span data-ttu-id="bfda4-746">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="bfda4-746">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="bfda4-747">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-747">**Why**</span></span>

<span data-ttu-id="bfda4-748">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="bfda4-748">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="bfda4-749">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="bfda4-749">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bfda4-750">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-750">**Mitigations**</span></span>

<span data-ttu-id="bfda4-751">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="bfda4-751">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="bfda4-752">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="bfda4-752">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="bfda4-753">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="bfda4-753">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bfda4-754">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="bfda4-754">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bfda4-755">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="bfda4-755">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="bfda4-756">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-756">**Old behavior**</span></span>

<span data-ttu-id="bfda4-757">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="bfda4-757">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="bfda4-758">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="bfda4-758">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="bfda4-759">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-759">**New behavior**</span></span>

<span data-ttu-id="bfda4-760">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="bfda4-760">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="bfda4-761">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-761">**Why**</span></span>

<span data-ttu-id="bfda4-762">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="bfda4-762">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bfda4-763">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-763">**Mitigations**</span></span>

<span data-ttu-id="bfda4-764">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="bfda4-764">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="bfda4-765">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="bfda4-765">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="bfda4-766">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="bfda4-766">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="bfda4-767">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="bfda4-767">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="bfda4-768">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="bfda4-768">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="bfda4-769">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-769">**Old behavior**</span></span>

<span data-ttu-id="bfda4-770">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="bfda4-770">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="bfda4-771">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-771">**New behavior**</span></span>

<span data-ttu-id="bfda4-772">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="bfda4-772">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="bfda4-773">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-773">**Why**</span></span>

<span data-ttu-id="bfda4-774">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="bfda4-774">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="bfda4-775">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="bfda4-775">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="bfda4-776">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-776">**Mitigations**</span></span>

<span data-ttu-id="bfda4-777">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="bfda4-777">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="bfda4-778">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="bfda4-778">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="bfda4-779">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="bfda4-779">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="bfda4-780">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="bfda4-780">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="bfda4-781">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="bfda4-781">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="bfda4-782">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="bfda4-782">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="bfda4-783">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-783">**Old behavior**</span></span>

<span data-ttu-id="bfda4-784">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="bfda4-784">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="bfda4-785">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-785">**New behavior**</span></span>

<span data-ttu-id="bfda4-786">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bfda4-786">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="bfda4-787">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-787">**Why**</span></span>

<span data-ttu-id="bfda4-788">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="bfda4-788">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="bfda4-789">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-789">**Mitigations**</span></span>

<span data-ttu-id="bfda4-790">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="bfda4-790">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="bfda4-791">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/aspnet/EntityFrameworkCore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="bfda4-791">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="bfda4-792">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="bfda4-792">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="bfda4-793">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="bfda4-793">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="bfda4-794">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="bfda4-794">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="bfda4-795">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-795">**Old behavior**</span></span>

<span data-ttu-id="bfda4-796">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-796">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="bfda4-797">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-797">**New behavior**</span></span>

<span data-ttu-id="bfda4-798">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-798">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="bfda4-799">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-799">**Why**</span></span>

<span data-ttu-id="bfda4-800">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="bfda4-800">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="bfda4-801">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-801">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="bfda4-802">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-802">**Mitigations**</span></span>

<span data-ttu-id="bfda4-803">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="bfda4-803">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="bfda4-804">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-804">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="bfda4-805">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="bfda4-805">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="bfda4-806">Отслеживание вопроса № 10985</span><span class="sxs-lookup"><span data-stu-id="bfda4-806">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="bfda4-807">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-807">**Change**</span></span>

<span data-ttu-id="bfda4-808">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-808">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="bfda4-809">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-809">**Why**</span></span>

<span data-ttu-id="bfda4-810">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-810">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="bfda4-811">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-811">**Mitigations**</span></span>

<span data-ttu-id="bfda4-812">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="bfda4-812">Use the new name.</span></span> <span data-ttu-id="bfda4-813">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="bfda4-813">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="bfda4-814">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="bfda4-814">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="bfda4-815">Отслеживание вопроса № 10730</span><span class="sxs-lookup"><span data-stu-id="bfda4-815">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="bfda4-816">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-816">**Old behavior**</span></span>

<span data-ttu-id="bfda4-817">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="bfda4-817">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="bfda4-818">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="bfda4-819">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-819">**New behavior**</span></span>

<span data-ttu-id="bfda4-820">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="bfda4-820">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="bfda4-821">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-821">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="bfda4-822">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-822">**Why**</span></span>

<span data-ttu-id="bfda4-823">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="bfda4-823">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="bfda4-824">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-824">**Mitigations**</span></span>

<span data-ttu-id="bfda4-825">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="bfda4-825">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="bfda4-826">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="bfda4-826">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="bfda4-827">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="bfda4-827">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="bfda4-828">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-828">**Old behavior**</span></span>

<span data-ttu-id="bfda4-829">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="bfda4-829">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="bfda4-830">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-830">**New behavior**</span></span>

<span data-ttu-id="bfda4-831">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="bfda4-831">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="bfda4-832">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-832">**Why**</span></span>

<span data-ttu-id="bfda4-833">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="bfda4-833">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="bfda4-834">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="bfda4-834">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="bfda4-835">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-835">**Mitigations**</span></span>

<span data-ttu-id="bfda4-836">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="bfda4-836">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="bfda4-837">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="bfda4-837">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="bfda4-838">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="bfda4-838">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="bfda4-839">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-839">**Old behavior**</span></span>

<span data-ttu-id="bfda4-840">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="bfda4-840">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="bfda4-841">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-841">**New behavior**</span></span>

<span data-ttu-id="bfda4-842">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bfda4-842">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="bfda4-843">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="bfda4-843">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="bfda4-844">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-844">**Why**</span></span>

<span data-ttu-id="bfda4-845">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="bfda4-845">This package is only intended to be used at design time.</span></span> <span data-ttu-id="bfda4-846">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="bfda4-846">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="bfda4-847">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bfda4-847">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="bfda4-848">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-848">**Mitigations**</span></span>

<span data-ttu-id="bfda4-849">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="bfda4-849">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="bfda4-850">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="bfda4-850">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="bfda4-851">Такую явную ссылку необходимо добавить в любой проект, где требуются типы из пакета.</span><span class="sxs-lookup"><span data-stu-id="bfda4-851">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="bfda4-852">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bfda4-852">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="bfda4-853">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="bfda4-853">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="bfda4-854">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-854">**Old behavior**</span></span>

<span data-ttu-id="bfda4-855">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="bfda4-855">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="bfda4-856">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-856">**New behavior**</span></span>

<span data-ttu-id="bfda4-857">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bfda4-857">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bfda4-858">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-858">**Why**</span></span>

<span data-ttu-id="bfda4-859">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="bfda4-859">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="bfda4-860">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="bfda4-860">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="bfda4-861">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-861">**Mitigations**</span></span>

<span data-ttu-id="bfda4-862">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-862">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bfda4-863">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="bfda4-863">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="bfda4-864">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bfda4-864">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="bfda4-865">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="bfda4-865">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="bfda4-866">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-866">**Old behavior**</span></span>

<span data-ttu-id="bfda4-867">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="bfda4-867">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="bfda4-868">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-868">**New behavior**</span></span>

<span data-ttu-id="bfda4-869">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bfda4-869">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bfda4-870">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-870">**Why**</span></span>

<span data-ttu-id="bfda4-871">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="bfda4-871">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="bfda4-872">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-872">**Mitigations**</span></span>

<span data-ttu-id="bfda4-873">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="bfda4-873">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bfda4-874">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="bfda4-874">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="bfda4-875">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="bfda4-875">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="bfda4-876">Отслеживание вопроса № 15636</span><span class="sxs-lookup"><span data-stu-id="bfda4-876">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="bfda4-877">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-877">**Old behavior**</span></span>

<span data-ttu-id="bfda4-878">Пакет Microsoft.EntityFrameworkCore.SqlServer ранее зависел от System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bfda4-878">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="bfda4-879">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-879">**New behavior**</span></span>

<span data-ttu-id="bfda4-880">Мы обновили пакет, и теперь он зависит от Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bfda4-880">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="bfda4-881">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-881">**Why**</span></span>

<span data-ttu-id="bfda4-882">Microsoft.Data.SqlClient теперь является основным разрабатываемым драйвером для доступа к SQL Server вместо System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bfda4-882">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="bfda4-883">Некоторые важные функции, такие как Always Encrypted, доступны только в Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bfda4-883">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="bfda4-884">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-884">**Mitigations**</span></span>

<span data-ttu-id="bfda4-885">Если код включает прямую зависимость от System.Data.SqlClient, необходимо изменить код так, чтобы он ссылался на Microsoft.Data.SqlClient. Поскольку API двух пакетов очень хорошо совместимы, достаточно изменить пакет и пространство имен.</span><span class="sxs-lookup"><span data-stu-id="bfda4-885">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="bfda4-886">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="bfda4-886">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="bfda4-887">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="bfda4-887">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="bfda4-888">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-888">**Old behavior**</span></span>

<span data-ttu-id="bfda4-889">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="bfda4-889">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="bfda4-890">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-890">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="bfda4-891">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-891">**New behavior**</span></span>

<span data-ttu-id="bfda4-892">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="bfda4-892">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="bfda4-893">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-893">**Why**</span></span>

<span data-ttu-id="bfda4-894">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="bfda4-894">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="bfda4-895">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-895">**Mitigations**</span></span>

<span data-ttu-id="bfda4-896">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="bfda4-896">Use full configuration of the relationship.</span></span> <span data-ttu-id="bfda4-897">Пример:</span><span class="sxs-lookup"><span data-stu-id="bfda4-897">For example:</span></span>

```csharp
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="bfda4-898">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="bfda4-898">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="bfda4-899">Отслеживание вопроса № 12757</span><span class="sxs-lookup"><span data-stu-id="bfda4-899">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="bfda4-900">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-900">**Old behavior**</span></span>

<span data-ttu-id="bfda4-901">DbFunction, настроенная со схемой в виде пустой строки, обрабатывалась как встроенная функция без схемы.</span><span class="sxs-lookup"><span data-stu-id="bfda4-901">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="bfda4-902">Например, следующий код сопоставит функцию CLR `DatePart` со встроенной функцией `DATEPART` в SqlServer.</span><span class="sxs-lookup"><span data-stu-id="bfda4-902">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="bfda4-903">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="bfda4-903">**New behavior**</span></span>

<span data-ttu-id="bfda4-904">Все сопоставления DbFunction считаются сопоставленными с определяемыми пользователем функциями.</span><span class="sxs-lookup"><span data-stu-id="bfda4-904">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="bfda4-905">Поэтому пустое строковое значение поместит функцию в схему по умолчанию для модели.</span><span class="sxs-lookup"><span data-stu-id="bfda4-905">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="bfda4-906">Это может быть схема, настроенная явным образом с помощью `modelBuilder.HasDefaultSchema()` текучего API либо `dbo`.</span><span class="sxs-lookup"><span data-stu-id="bfda4-906">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="bfda4-907">**Причина**</span><span class="sxs-lookup"><span data-stu-id="bfda4-907">**Why**</span></span>

<span data-ttu-id="bfda4-908">Ранее эта функция считалась встроенной из-за пустой схемы, но эта логика применима только к SqlServer, где встроенные функции не принадлежат ни одной схеме.</span><span class="sxs-lookup"><span data-stu-id="bfda4-908">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="bfda4-909">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="bfda4-909">**Mitigations**</span></span>

<span data-ttu-id="bfda4-910">Вручную настройте преобразование функции DbFunction, чтобы сопоставить ее со встроенной функцией.</span><span class="sxs-lookup"><span data-stu-id="bfda4-910">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
