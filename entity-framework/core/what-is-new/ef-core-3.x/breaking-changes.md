---
title: Критические изменения в EF Core 3.x — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 3.x
author: ajcvickers
ms.date: 09/05/2020
uid: core/what-is-new/ef-core-3.x/breaking-changes
ms.openlocfilehash: bfcfb7257091d1b6889f7c0af00ddab10e0e12e3
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429316"
---
# <a name="breaking-changes-included-in-ef-core-3x"></a><span data-ttu-id="866a3-103">Критические изменения в EF Core 3.x</span><span class="sxs-lookup"><span data-stu-id="866a3-103">Breaking changes included in EF Core 3.x</span></span>

<span data-ttu-id="866a3-104">Указанные ниже изменения API и поведения могут нарушать работу существующих приложений при их обновлении до версии 3.x.</span><span class="sxs-lookup"><span data-stu-id="866a3-104">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.x.</span></span>
<span data-ttu-id="866a3-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="866a3-105">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="866a3-106">Сводка</span><span class="sxs-lookup"><span data-stu-id="866a3-106">Summary</span></span>

| <span data-ttu-id="866a3-107">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="866a3-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="866a3-108">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="866a3-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="866a3-109">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="866a3-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="866a3-110">Высокий</span><span class="sxs-lookup"><span data-stu-id="866a3-110">High</span></span>       |
| [<span data-ttu-id="866a3-111">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="866a3-111">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="866a3-112">Высокий</span><span class="sxs-lookup"><span data-stu-id="866a3-112">High</span></span>      |
| [<span data-ttu-id="866a3-113">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="866a3-113">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="866a3-114">Высокий</span><span class="sxs-lookup"><span data-stu-id="866a3-114">High</span></span>      |
| [<span data-ttu-id="866a3-115">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="866a3-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="866a3-116">Высокий</span><span class="sxs-lookup"><span data-stu-id="866a3-116">High</span></span>      |
| [<span data-ttu-id="866a3-117">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="866a3-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="866a3-118">Высокий</span><span class="sxs-lookup"><span data-stu-id="866a3-118">High</span></span>      |
| [<span data-ttu-id="866a3-119">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="866a3-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="866a3-120">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-120">Medium</span></span>      |
| [<span data-ttu-id="866a3-121">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="866a3-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="866a3-122">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-122">Medium</span></span>      |
| [<span data-ttu-id="866a3-123">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="866a3-123">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="866a3-124">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-124">Medium</span></span>      |
| [<span data-ttu-id="866a3-125">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="866a3-125">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="866a3-126">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-126">Medium</span></span>      |
| [<span data-ttu-id="866a3-127">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="866a3-127">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="866a3-128">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-128">Medium</span></span>      |
| [<span data-ttu-id="866a3-129">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="866a3-129">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="866a3-130">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-130">Medium</span></span>      |
| [<span data-ttu-id="866a3-131">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="866a3-131">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="866a3-132">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-132">Medium</span></span>      |
| [<span data-ttu-id="866a3-133">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="866a3-133">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="866a3-134">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-134">Medium</span></span>      |
| [<span data-ttu-id="866a3-135">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="866a3-135">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="866a3-136">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-136">Medium</span></span>      |
| [<span data-ttu-id="866a3-137">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="866a3-137">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="866a3-138">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-138">Medium</span></span>      |
| [<span data-ttu-id="866a3-139">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="866a3-139">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="866a3-140">Средний</span><span class="sxs-lookup"><span data-stu-id="866a3-140">Medium</span></span>      |
| [<span data-ttu-id="866a3-141">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="866a3-141">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="866a3-142">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-142">Low</span></span>      |
| [<span data-ttu-id="866a3-143">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="866a3-143">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="866a3-144">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-144">Low</span></span>      |
| [<span data-ttu-id="866a3-145">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="866a3-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="866a3-146">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-146">Low</span></span>      |
| [<span data-ttu-id="866a3-147">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="866a3-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="866a3-148">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-148">Low</span></span>      |
| [<span data-ttu-id="866a3-149">Собственные сущности нельзя запрашивать без использования запроса отслеживания владельцем</span><span class="sxs-lookup"><span data-stu-id="866a3-149">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="866a3-150">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-150">Low</span></span>      |
| [<span data-ttu-id="866a3-151">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="866a3-151">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="866a3-152">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-152">Low</span></span>      |
| [<span data-ttu-id="866a3-153">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="866a3-153">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="866a3-154">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-154">Low</span></span>      |
| [<span data-ttu-id="866a3-155">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="866a3-155">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="866a3-156">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-156">Low</span></span>      |
| [<span data-ttu-id="866a3-157">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="866a3-157">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="866a3-158">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-158">Low</span></span>      |
| [<span data-ttu-id="866a3-159">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="866a3-159">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="866a3-160">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-160">Low</span></span>      |
| [<span data-ttu-id="866a3-161">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="866a3-161">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="866a3-162">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-162">Low</span></span>      |
| [<span data-ttu-id="866a3-163">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="866a3-163">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="866a3-164">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-164">Low</span></span>      |
| [<span data-ttu-id="866a3-165">AddEntityFramework\* добавляет IMemoryCache с ограниченным размером</span><span class="sxs-lookup"><span data-stu-id="866a3-165">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="866a3-166">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-166">Low</span></span>      |
| [<span data-ttu-id="866a3-167">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="866a3-167">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="866a3-168">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-168">Low</span></span>      |
| [<span data-ttu-id="866a3-169">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="866a3-169">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="866a3-170">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-170">Low</span></span>      |
| [<span data-ttu-id="866a3-171">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="866a3-171">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="866a3-172">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-172">Low</span></span>      |
| [<span data-ttu-id="866a3-173">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="866a3-173">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="866a3-174">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-174">Low</span></span>      |
| [<span data-ttu-id="866a3-175">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="866a3-175">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="866a3-176">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-176">Low</span></span>      |
| [<span data-ttu-id="866a3-177">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="866a3-177">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="866a3-178">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-178">Low</span></span>      |
| [<span data-ttu-id="866a3-179">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="866a3-179">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="866a3-180">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-180">Low</span></span>      |
| [<span data-ttu-id="866a3-181">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="866a3-181">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="866a3-182">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-182">Low</span></span>      |
| [<span data-ttu-id="866a3-183">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="866a3-183">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="866a3-184">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-184">Low</span></span>      |
| [<span data-ttu-id="866a3-185">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="866a3-185">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="866a3-186">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-186">Low</span></span>      |
| [<span data-ttu-id="866a3-187">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="866a3-187">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="866a3-188">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-188">Low</span></span>      |
| [<span data-ttu-id="866a3-189">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="866a3-189">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="866a3-190">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-190">Low</span></span>      |
| [<span data-ttu-id="866a3-191">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="866a3-191">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="866a3-192">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-192">Low</span></span>      |
| [<span data-ttu-id="866a3-193">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="866a3-193">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="866a3-194">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-194">Low</span></span>      |
| [<span data-ttu-id="866a3-195">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="866a3-195">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="866a3-196">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-196">Low</span></span>      |
| [<span data-ttu-id="866a3-197">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="866a3-197">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="866a3-198">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-198">Low</span></span>      |
| [<span data-ttu-id="866a3-199">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="866a3-199">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="866a3-200">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-200">Low</span></span>      |
| [<span data-ttu-id="866a3-201">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="866a3-201">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="866a3-202">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-202">Low</span></span>      |
| [<span data-ttu-id="866a3-203">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="866a3-203">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="866a3-204">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-204">Low</span></span>      |
| [<span data-ttu-id="866a3-205">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="866a3-205">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="866a3-206">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-206">Low</span></span>      |
| [<span data-ttu-id="866a3-207">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="866a3-207">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="866a3-208">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-208">Low</span></span>      |
| [<span data-ttu-id="866a3-209">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="866a3-209">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="866a3-210">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-210">Low</span></span>      |
| [<span data-ttu-id="866a3-211">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="866a3-211">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="866a3-212">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-212">Low</span></span>      |
| [<span data-ttu-id="866a3-213">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="866a3-213">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="866a3-214">Низкий</span><span class="sxs-lookup"><span data-stu-id="866a3-214">Low</span></span>      |
| [<span data-ttu-id="866a3-215">~~Версия EF Core 3.0 больше предназначена для .NET Standard 2.1, а не для .NET Standard 2.0~~ отменено</span><span class="sxs-lookup"><span data-stu-id="866a3-215">~~EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0~~ Reverted</span></span>](#netstandard21) | |
| [<span data-ttu-id="866a3-216">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="866a3-216">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | |

## <a name="high-impact-changes"></a><span data-ttu-id="866a3-217">Изменения высокой степени влияния</span><span class="sxs-lookup"><span data-stu-id="866a3-217">High-impact changes</span></span>

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="866a3-218">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="866a3-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="866a3-219">[Отслеживание вопроса № 14935](https://github.com/dotnet/efcore/issues/14935)
[Также см. вопрос № 12795](https://github.com/dotnet/efcore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="866a3-219">[Tracking Issue #14935](https://github.com/dotnet/efcore/issues/14935)
[Also see issue #12795](https://github.com/dotnet/efcore/issues/12795)</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="866a3-220">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-220">Old behavior</span></span>

<span data-ttu-id="866a3-221">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="866a3-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="866a3-222">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="866a3-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-223">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-223">New behavior</span></span>

<span data-ttu-id="866a3-224">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="866a3-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="866a3-225">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="866a3-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-226">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-226">Why</span></span>

<span data-ttu-id="866a3-227">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="866a3-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="866a3-228">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="866a3-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="866a3-229">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="866a3-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="866a3-230">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="866a3-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="866a3-231">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="866a3-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="866a3-232">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="866a3-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-233">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-233">Mitigations</span></span>

<span data-ttu-id="866a3-234">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="866a3-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>

## <a name="medium-impact-changes"></a><span data-ttu-id="866a3-235">Изменения средней степени влияния</span><span class="sxs-lookup"><span data-stu-id="866a3-235">Medium-impact changes</span></span>

### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="866a3-236">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="866a3-236">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="866a3-237">Отслеживание объявлений о вопросе № 325</span><span class="sxs-lookup"><span data-stu-id="866a3-237">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-238">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-238">Old behavior</span></span>

<span data-ttu-id="866a3-239">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="866a3-239">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-240">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-240">New behavior</span></span>

<span data-ttu-id="866a3-241">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-241">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-242">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-242">Why</span></span>

<span data-ttu-id="866a3-243">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="866a3-243">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span>
<span data-ttu-id="866a3-244">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="866a3-244">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="866a3-245">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="866a3-245">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="866a3-246">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-246">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-247">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-247">Mitigations</span></span>

<span data-ttu-id="866a3-248">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="866a3-248">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>

### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="866a3-249">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="866a3-249">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="866a3-250">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="866a3-250">Tracking Issue #14016</span></span>](https://github.com/dotnet/efcore/issues/14016)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-251">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-251">Old behavior</span></span>

<span data-ttu-id="866a3-252">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="866a3-252">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-253">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-253">New behavior</span></span>

<span data-ttu-id="866a3-254">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="866a3-254">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-255">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-255">Why</span></span>

<span data-ttu-id="866a3-256">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="866a3-256">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-257">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-257">Mitigations</span></span>

<span data-ttu-id="866a3-258">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="866a3-258">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="866a3-259">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](/dotnet/core/tools/global-tools#install-a-local-tool).</span><span class="sxs-lookup"><span data-stu-id="866a3-259">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](/dotnet/core/tools/global-tools#install-a-local-tool).</span></span>

## <a name="low-impact-changes"></a><span data-ttu-id="866a3-260">Изменения низкой степени влияния</span><span class="sxs-lookup"><span data-stu-id="866a3-260">Low-impact changes</span></span>

<a name="fromsql"></a>

### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="866a3-261">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="866a3-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="866a3-262">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="866a3-262">Tracking Issue #10996</span></span>](https://github.com/dotnet/efcore/issues/10996)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-263">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-263">Old behavior</span></span>

<span data-ttu-id="866a3-264">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="866a3-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-265">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-265">New behavior</span></span>

<span data-ttu-id="866a3-266">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="866a3-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="866a3-267">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-267">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="866a3-268">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="866a3-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="866a3-269">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-269">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="866a3-270">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="866a3-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-271">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-271">Why</span></span>

<span data-ttu-id="866a3-272">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="866a3-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="866a3-273">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="866a3-273">This could result in queries not being parameterized when they should have been.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-274">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-274">Mitigations</span></span>

<span data-ttu-id="866a3-275">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="866a3-275">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>

### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="866a3-276">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="866a3-276">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="866a3-277">Отслеживание вопроса № 15392</span><span class="sxs-lookup"><span data-stu-id="866a3-277">Tracking Issue #15392</span></span>](https://github.com/dotnet/efcore/issues/15392)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-278">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-278">Old behavior</span></span>

<span data-ttu-id="866a3-279">До версии EF Core 3.0 метод FromSql пытался определить, можно ли выполнить составление для переданного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="866a3-279">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="866a3-280">Если SQL-запрос не допускал составления, как в случае с хранимой процедурой, вычисление производилось на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="866a3-280">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="866a3-281">В случае с приведенным ниже запросом хранимая процедура выполнялась на сервере, а метод FirstOrDefault — на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="866a3-281">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

#### <a name="new-behavior"></a><span data-ttu-id="866a3-282">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-282">New behavior</span></span>

<span data-ttu-id="866a3-283">Начиная с версии 3.0 платформа EF Core не пытается анализировать код SQL.</span><span class="sxs-lookup"><span data-stu-id="866a3-283">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="866a3-284">Поэтому если вы выполняете составление после метода FromSqlRaw или FromSqlInterpolated, EF Core составляет SQL-запрос, вызывая вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="866a3-284">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="866a3-285">При использовании хранимой процедуры с составлением вы получите исключение, вызванное недопустимым синтаксисом SQL.</span><span class="sxs-lookup"><span data-stu-id="866a3-285">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-286">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-286">Why</span></span>

<span data-ttu-id="866a3-287">EF Core 3.0 не поддерживает автоматическое вычисление на стороне клиента, так как оно было подвержено ошибкам, как описано [здесь](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="866a3-287">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-288">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-288">Mitigations</span></span>

<span data-ttu-id="866a3-289">Если вы используете хранимую процедуру в методе FromSqlRaw или FromSqlInterpolated, то знаете, что составление для нее невозможно, поэтому вы можете добавить __AsEnumerable или AsAsyncEnumerable__ сразу после вызова метода FromSql, чтобы избежать составления на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="866a3-289">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="866a3-290">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="866a3-290">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="866a3-291">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="866a3-291">Tracking Issue #15704</span></span>](https://github.com/dotnet/efcore/issues/15704)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-292">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-292">Old behavior</span></span>

<span data-ttu-id="866a3-293">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="866a3-293">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-294">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-294">New behavior</span></span>

<span data-ttu-id="866a3-295">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="866a3-295">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="866a3-296">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="866a3-296">Attempting to specify them anywhere else will result in a compilation error.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-297">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-297">Why</span></span>

<span data-ttu-id="866a3-298">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="866a3-298">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-299">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-299">Mitigations</span></span>

<span data-ttu-id="866a3-300">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="866a3-300">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>

### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="866a3-301">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="866a3-301">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="866a3-302">Отслеживание вопроса № 13518</span><span class="sxs-lookup"><span data-stu-id="866a3-302">Tracking Issue #13518</span></span>](https://github.com/dotnet/efcore/issues/13518)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-303">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-303">Old behavior</span></span>

<span data-ttu-id="866a3-304">До EF Core 3.0 один и тот же экземпляр сущности будет использоваться для каждого вхождения сущности с заданным типом и ИД.</span><span class="sxs-lookup"><span data-stu-id="866a3-304">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="866a3-305">Это соответствует поведению запросов отслеживания.</span><span class="sxs-lookup"><span data-stu-id="866a3-305">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="866a3-306">Например, следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="866a3-306">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```

<span data-ttu-id="866a3-307">возвращает тот же экземпляр `Category` для каждого `Product`, связанного с заданной категорией.</span><span class="sxs-lookup"><span data-stu-id="866a3-307">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-308">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-308">New behavior</span></span>

<span data-ttu-id="866a3-309">Начиная с EF Core 3.0, при обнаружении сущности с заданным типом и ИД в разных местах возвращенного графа будут создаваться различные экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="866a3-309">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="866a3-310">Например, вышеприведенный запрос теперь будет возвращать новый экземпляр `Category` для каждого `Product`, даже если два продукта связаны с одной категорией.</span><span class="sxs-lookup"><span data-stu-id="866a3-310">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-311">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-311">Why</span></span>

<span data-ttu-id="866a3-312">Разрешение идентификаторов (то есть определение того, что сущность имеет тот же тип и идентификатор, что и обнаруженная ранее сущность) добавляет дополнительную нагрузку на производительность и память.</span><span class="sxs-lookup"><span data-stu-id="866a3-312">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="866a3-313">Обычно это идет вразрез с причинами использования запросов без отслеживания.</span><span class="sxs-lookup"><span data-stu-id="866a3-313">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="866a3-314">Кроме того, хотя разрешение идентификаторов иногда может быть полезным, оно не требуется, если сущности должны быть сериализованы и отправлены клиенту, что является распространенным случаем для неотслеживаемых запросов.</span><span class="sxs-lookup"><span data-stu-id="866a3-314">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-315">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-315">Mitigations</span></span>

<span data-ttu-id="866a3-316">Используйте запрос с отслеживанием, если требуется разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="866a3-316">Use a tracking query if identity resolution is required.</span></span>

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="866a3-317">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="866a3-317">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="866a3-318">Отслеживание вопроса № 12378</span><span class="sxs-lookup"><span data-stu-id="866a3-318">Tracking Issue #12378</span></span>](https://github.com/dotnet/efcore/issues/12378)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-319">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-319">Old behavior</span></span>

<span data-ttu-id="866a3-320">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="866a3-320">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="866a3-321">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="866a3-321">Usually these temporary values were large negative numbers.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-322">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-322">New behavior</span></span>

<span data-ttu-id="866a3-323">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="866a3-323">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-324">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-324">Why</span></span>

<span data-ttu-id="866a3-325">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="866a3-325">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-326">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-326">Mitigations</span></span>

<span data-ttu-id="866a3-327">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="866a3-327">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="866a3-328">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="866a3-328">This can be avoided by:</span></span>

* <span data-ttu-id="866a3-329">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="866a3-329">Not using store-generated keys.</span></span>
* <span data-ttu-id="866a3-330">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="866a3-330">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="866a3-331">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="866a3-331">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="866a3-332">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="866a3-332">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="866a3-333">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="866a3-333">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="866a3-334">Отслеживание вопроса № 14616</span><span class="sxs-lookup"><span data-stu-id="866a3-334">Tracking Issue #14616</span></span>](https://github.com/dotnet/efcore/issues/14616)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-335">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-335">Old behavior</span></span>

<span data-ttu-id="866a3-336">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="866a3-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-337">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-337">New behavior</span></span>

<span data-ttu-id="866a3-338">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="866a3-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="866a3-339">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="866a3-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="866a3-340">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="866a3-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-341">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-341">Why</span></span>

<span data-ttu-id="866a3-342">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="866a3-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-343">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-343">Mitigations</span></span>

<span data-ttu-id="866a3-344">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="866a3-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="866a3-345">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="866a3-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="866a3-346">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="866a3-346">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="866a3-347">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="866a3-347">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

<a name="cascade"></a>

### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="866a3-348">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="866a3-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="866a3-349">Отслеживание вопроса № 10114</span><span class="sxs-lookup"><span data-stu-id="866a3-349">Tracking Issue #10114</span></span>](https://github.com/dotnet/efcore/issues/10114)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-350">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-350">Old behavior</span></span>

<span data-ttu-id="866a3-351">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="866a3-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-352">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-352">New behavior</span></span>

<span data-ttu-id="866a3-353">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="866a3-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="866a3-354">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="866a3-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-355">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-355">Why</span></span>

<span data-ttu-id="866a3-356">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед вызовом_  `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="866a3-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-357">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-357">Mitigations</span></span>

<span data-ttu-id="866a3-358">Для восстановления прежнего поведения можно использовать параметры `context.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="866a3-358">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="866a3-359">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-359">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

<a name="eager-loading-single-query"></a>

### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="866a3-360">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="866a3-360">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="866a3-361">Отслеживание вопроса № 18022</span><span class="sxs-lookup"><span data-stu-id="866a3-361">Tracking issue #18022</span></span>](https://github.com/dotnet/efcore/issues/18022)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-362">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-362">Old behavior</span></span>

<span data-ttu-id="866a3-363">До версии 3.0 безотложная загрузка навигаций по коллекциям с помощью операторов `Include` приводила к созданию нескольких запросов к реляционной базе данных, по одной для каждого связанного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="866a3-363">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-364">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-364">New behavior</span></span>

<span data-ttu-id="866a3-365">Начиная с версии 3.0 платформа EF Core создает один запрос с соединениями к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="866a3-365">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-366">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-366">Why</span></span>

<span data-ttu-id="866a3-367">Отправка нескольких запросов для реализации одного запроса LINQ приводила ко множеству проблем, включая снижение производительности из-за необходимости нескольких циклов обращения к базе данных и проблем с согласованностью данных, так как при каждом запросе состояние базы данных могло быть разным.</span><span class="sxs-lookup"><span data-stu-id="866a3-367">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-368">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-368">Mitigations</span></span>

<span data-ttu-id="866a3-369">Хотя технически это изменение не является критическим, оно может существенно повлиять на производительность приложения в случае, если один запрос содержит большое число операторов `Include` с навигациями по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="866a3-369">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="866a3-370">Дополнительные сведения и указания по повышению эффективности запросов см. в [этом комментарии](https://github.com/dotnet/efcore/issues/18022#issuecomment-542397085).</span><span class="sxs-lookup"><span data-stu-id="866a3-370">[See this comment](https://github.com/dotnet/efcore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>

### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="866a3-371">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="866a3-371">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="866a3-372">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="866a3-372">Tracking Issue #12661</span></span>](https://github.com/dotnet/efcore/issues/12661)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-373">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-373">Old behavior</span></span>

<span data-ttu-id="866a3-374">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="866a3-374">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-375">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-375">New behavior</span></span>

<span data-ttu-id="866a3-376">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="866a3-376">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-377">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-377">Why</span></span>

<span data-ttu-id="866a3-378">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="866a3-378">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-379">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-379">Mitigations</span></span>

<span data-ttu-id="866a3-380">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="866a3-380">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>

### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="866a3-381">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="866a3-381">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="866a3-382">Отслеживание вопроса № 14194</span><span class="sxs-lookup"><span data-stu-id="866a3-382">Tracking Issue #14194</span></span>](https://github.com/dotnet/efcore/issues/14194)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-383">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-383">Old behavior</span></span>

<span data-ttu-id="866a3-384">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/keyless-entity-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="866a3-384">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="866a3-385">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="866a3-385">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-386">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-386">New behavior</span></span>

<span data-ttu-id="866a3-387">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="866a3-387">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="866a3-388">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="866a3-388">Keyless entity types have the same functionality as query types in previous versions.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-389">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-389">Why</span></span>

<span data-ttu-id="866a3-390">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="866a3-390">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="866a3-391">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="866a3-391">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="866a3-392">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="866a3-392">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-393">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-393">Mitigations</span></span>

<span data-ttu-id="866a3-394">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="866a3-394">The following parts of the API are now obsolete:</span></span>

* <span data-ttu-id="866a3-395">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="866a3-395">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="866a3-396">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="866a3-396">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="866a3-397">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="866a3-397">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="866a3-398">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="866a3-398">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>
* <span data-ttu-id="866a3-399">**`IQueryTypeConfiguration<TQuery>`** — вместо него следует использовать `IEntityTypeConfiguration<TEntity>`.</span><span class="sxs-lookup"><span data-stu-id="866a3-399">**`IQueryTypeConfiguration<TQuery>`** - Instead `IEntityTypeConfiguration<TEntity>` should be used.</span></span>

> [!NOTE]
> <span data-ttu-id="866a3-400">Из-за [проблемы в версиях 3.x](https://github.com/dotnet/efcore/issues/19537) при запросе сущностей без ключей, все свойства которых имеют значение `null`, возвращается `null` вместо сущности. Если эта проблема затрагивает ваш сценарий, также добавьте логику для обработки `null` в результатах.</span><span class="sxs-lookup"><span data-stu-id="866a3-400">Due to [an issue in 3.x](https://github.com/dotnet/efcore/issues/19537) when querying keyless entities that have all properties set to `null` a `null` will be returned instead of an entity, if this issue is applicable to your scenario also add logic to handle `null` in results.</span></span>

<a name="config"></a>

### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="866a3-401">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="866a3-401">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="866a3-402">[Отслеживание вопроса 12444](https://github.com/dotnet/efcore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/dotnet/efcore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/dotnet/efcore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="866a3-402">[Tracking Issue #12444](https://github.com/dotnet/efcore/issues/12444)
[Tracking Issue #9148](https://github.com/dotnet/efcore/issues/9148)
[Tracking Issue #14153](https://github.com/dotnet/efcore/issues/14153)</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="866a3-403">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-403">Old behavior</span></span>

<span data-ttu-id="866a3-404">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="866a3-404">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-405">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-405">New behavior</span></span>

<span data-ttu-id="866a3-406">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="866a3-406">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="866a3-407">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-407">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="866a3-408">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="866a3-408">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="866a3-409">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="866a3-409">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="866a3-410">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-410">For example:</span></span>

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

<span data-ttu-id="866a3-411">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="866a3-411">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-412">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-412">Why</span></span>

<span data-ttu-id="866a3-413">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="866a3-413">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="866a3-414">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="866a3-414">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-415">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-415">Mitigations</span></span>

<span data-ttu-id="866a3-416">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="866a3-416">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="866a3-417">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="866a3-417">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="866a3-418">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="866a3-418">Tracking Issue #9005</span></span>](https://github.com/dotnet/efcore/issues/9005)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-419">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-419">Old behavior</span></span>

<span data-ttu-id="866a3-420">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="866a3-420">Consider the following model:</span></span>

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

<span data-ttu-id="866a3-421">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="866a3-421">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-422">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-422">New behavior</span></span>

<span data-ttu-id="866a3-423">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="866a3-423">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="866a3-424">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="866a3-424">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-425">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-425">Mitigations</span></span>

<span data-ttu-id="866a3-426">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="866a3-426">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="866a3-427">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="866a3-427">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="866a3-428">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="866a3-428">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="866a3-429">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="866a3-429">Tracking Issue #14154</span></span>](https://github.com/dotnet/efcore/issues/14154)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-430">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-430">Old behavior</span></span>

<span data-ttu-id="866a3-431">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="866a3-431">Consider the following model:</span></span>

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

<span data-ttu-id="866a3-432">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="866a3-432">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-433">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-433">New behavior</span></span>

<span data-ttu-id="866a3-434">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="866a3-434">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="866a3-435">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="866a3-435">Otherwise an exception is thrown during model validation.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-436">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-436">Why</span></span>

<span data-ttu-id="866a3-437">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="866a3-437">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-438">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-438">Mitigations</span></span>

<span data-ttu-id="866a3-439">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="866a3-439">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="866a3-440">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="866a3-440">It's possible the create one in shadow-state:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="866a3-441">Собственные сущности нельзя запрашивать без использования запроса отслеживания владельцем</span><span class="sxs-lookup"><span data-stu-id="866a3-441">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="866a3-442">Отслеживание вопроса № 18876</span><span class="sxs-lookup"><span data-stu-id="866a3-442">Tracking Issue #18876</span></span>](https://github.com/dotnet/efcore/issues/18876)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-443">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-443">Old behavior</span></span>

<span data-ttu-id="866a3-444">До версии EF Core 3.0 собственные сущности можно было запрашивать как любую другую навигацию.</span><span class="sxs-lookup"><span data-stu-id="866a3-444">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

#### <a name="new-behavior"></a><span data-ttu-id="866a3-445">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-445">New behavior</span></span>

<span data-ttu-id="866a3-446">Начиная с версии 3.0 EF Core выдает исключение, если запрос отслеживания проецирует собственную сущность без владельца.</span><span class="sxs-lookup"><span data-stu-id="866a3-446">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-447">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-447">Why</span></span>

<span data-ttu-id="866a3-448">Выполнять операции с собственными сущностями без владельца невозможно, поэтому в подавляющем большинстве случаев подобные запросы к ним являются ошибкой.</span><span class="sxs-lookup"><span data-stu-id="866a3-448">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-449">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-449">Mitigations</span></span>

<span data-ttu-id="866a3-450">Если собственную сущность следует отслеживать для изменения в последующем, владелец должен быть включен в запрос.</span><span class="sxs-lookup"><span data-stu-id="866a3-450">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="866a3-451">В противном случае добавьте вызов `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="866a3-451">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="866a3-452">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="866a3-452">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="866a3-453">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="866a3-453">Tracking Issue #13998</span></span>](https://github.com/dotnet/efcore/issues/13998)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-454">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-454">Old behavior</span></span>

<span data-ttu-id="866a3-455">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="866a3-455">Consider the following model:</span></span>

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

<span data-ttu-id="866a3-456">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="866a3-456">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-457">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-457">New behavior</span></span>

<span data-ttu-id="866a3-458">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="866a3-458">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-459">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-459">Why</span></span>

<span data-ttu-id="866a3-460">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="866a3-460">The old behavoir was unexpected.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-461">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-461">Mitigations</span></span>

<span data-ttu-id="866a3-462">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="866a3-462">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="866a3-463">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="866a3-463">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="866a3-464">Отслеживание вопроса № 13274</span><span class="sxs-lookup"><span data-stu-id="866a3-464">Tracking Issue #13274</span></span>](https://github.com/dotnet/efcore/issues/13274)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-465">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-465">Old behavior</span></span>

<span data-ttu-id="866a3-466">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="866a3-466">Consider the following model:</span></span>

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

<span data-ttu-id="866a3-467">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="866a3-467">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="866a3-468">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="866a3-468">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-469">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-469">New behavior</span></span>

<span data-ttu-id="866a3-470">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="866a3-470">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="866a3-471">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="866a3-471">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="866a3-472">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-472">For example:</span></span>

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

#### <a name="why"></a><span data-ttu-id="866a3-473">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-473">Why</span></span>

<span data-ttu-id="866a3-474">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="866a3-474">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-475">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-475">Mitigations</span></span>

<span data-ttu-id="866a3-476">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="866a3-476">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="866a3-477">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="866a3-477">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="866a3-478">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="866a3-478">Tracking Issue #14218</span></span>](https://github.com/dotnet/efcore/issues/14218)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-479">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-479">Old behavior</span></span>

<span data-ttu-id="866a3-480">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="866a3-480">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

#### <a name="new-behavior"></a><span data-ttu-id="866a3-481">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-481">New behavior</span></span>

<span data-ttu-id="866a3-482">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="866a3-482">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-483">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-483">Why</span></span>

<span data-ttu-id="866a3-484">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="866a3-484">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="866a3-485">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="866a3-485">The new behavior also matches EF6.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-486">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-486">Mitigations</span></span>

<span data-ttu-id="866a3-487">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="866a3-487">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="866a3-488">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="866a3-488">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="866a3-489">Отслеживание вопроса № 6872</span><span class="sxs-lookup"><span data-stu-id="866a3-489">Tracking Issue #6872</span></span>](https://github.com/dotnet/efcore/issues/6872)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-490">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-490">Old behavior</span></span>

<span data-ttu-id="866a3-491">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="866a3-491">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-492">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-492">New behavior</span></span>

<span data-ttu-id="866a3-493">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="866a3-493">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="866a3-494">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="866a3-494">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-495">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-495">Why</span></span>

<span data-ttu-id="866a3-496">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="866a3-496">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-497">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-497">Mitigations</span></span>

<span data-ttu-id="866a3-498">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="866a3-498">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="866a3-499">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="866a3-499">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="866a3-500">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="866a3-500">Backing fields are used by default</span></span>

[<span data-ttu-id="866a3-501">Отслеживание вопроса № 12430</span><span class="sxs-lookup"><span data-stu-id="866a3-501">Tracking Issue #12430</span></span>](https://github.com/dotnet/efcore/issues/12430)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-502">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-502">Old behavior</span></span>

<span data-ttu-id="866a3-503">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="866a3-503">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="866a3-504">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="866a3-504">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-505">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-505">New behavior</span></span>

<span data-ttu-id="866a3-506">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="866a3-506">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="866a3-507">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="866a3-507">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-508">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-508">Why</span></span>

<span data-ttu-id="866a3-509">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="866a3-509">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-510">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-510">Mitigations</span></span>

<span data-ttu-id="866a3-511">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="866a3-511">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="866a3-512">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-512">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="866a3-513">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="866a3-513">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="866a3-514">Отслеживание вопроса № 12523</span><span class="sxs-lookup"><span data-stu-id="866a3-514">Tracking Issue #12523</span></span>](https://github.com/dotnet/efcore/issues/12523)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-515">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-515">Old behavior</span></span>

<span data-ttu-id="866a3-516">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="866a3-516">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="866a3-517">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="866a3-517">This could cause the wrong field to be used in ambiguous cases.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-518">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-518">New behavior</span></span>

<span data-ttu-id="866a3-519">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="866a3-519">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-520">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-520">Why</span></span>

<span data-ttu-id="866a3-521">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="866a3-521">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-522">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-522">Mitigations</span></span>

<span data-ttu-id="866a3-523">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="866a3-523">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="866a3-524">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="866a3-524">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="866a3-525">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="866a3-525">Field-only property names should match the field name</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="866a3-526">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-526">Old behavior</span></span>

<span data-ttu-id="866a3-527">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе .NET, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="866a3-527">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

#### <a name="new-behavior"></a><span data-ttu-id="866a3-528">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-528">New behavior</span></span>

<span data-ttu-id="866a3-529">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="866a3-529">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

#### <a name="why"></a><span data-ttu-id="866a3-530">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-530">Why</span></span>

<span data-ttu-id="866a3-531">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="866a3-531">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-532">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-532">Mitigations</span></span>

<span data-ttu-id="866a3-533">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="866a3-533">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="866a3-534">В будущем выпуске EF Core после версии 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства (см. вопрос [№ 15307](https://github.com/dotnet/efcore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="866a3-534">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/dotnet/efcore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="866a3-535">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="866a3-535">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="866a3-536">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="866a3-536">Tracking Issue #14756</span></span>](https://github.com/dotnet/efcore/issues/14756)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-537">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-537">Old behavior</span></span>

<span data-ttu-id="866a3-538">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов методов [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="866a3-538">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-539">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-539">New behavior</span></span>

<span data-ttu-id="866a3-540">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="866a3-540">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-541">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-541">Why</span></span>

<span data-ttu-id="866a3-542">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="866a3-542">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="866a3-543">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-543">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-544">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-544">Mitigations</span></span>

<span data-ttu-id="866a3-545">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="866a3-545">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="866a3-546">AddEntityFramework\* добавляет IMemoryCache с ограниченным размером</span><span class="sxs-lookup"><span data-stu-id="866a3-546">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="866a3-547">Отслеживание вопроса № 12905</span><span class="sxs-lookup"><span data-stu-id="866a3-547">Tracking Issue #12905</span></span>](https://github.com/dotnet/efcore/issues/12905)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-548">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-548">Old behavior</span></span>

<span data-ttu-id="866a3-549">До EF Core 3.0 вызов методов `AddEntityFramework*` также приводил к регистрации служб кэширования памяти с внедрением зависимостей без ограничения размера.</span><span class="sxs-lookup"><span data-stu-id="866a3-549">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-550">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-550">New behavior</span></span>

<span data-ttu-id="866a3-551">Начиная с версии EF Core 3.0 вызов `AddEntityFramework*` приводит к регистрации службы IMemoryCache с ограничением размера.</span><span class="sxs-lookup"><span data-stu-id="866a3-551">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="866a3-552">Если добавленные позже службы зависят от IMemoryCache, они могут быстро достичь ограничения по умолчанию, что приведет к созданию исключений или снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="866a3-552">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-553">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-553">Why</span></span>

<span data-ttu-id="866a3-554">Использование IMemoryCache без ограничения может привести к неконтролируемому использованию памяти, если в логике кэширования запросов есть ошибка или запросы создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="866a3-554">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="866a3-555">Ограничение по умолчанию позволяет предотвращать возможные атаки типа "отказ в обслуживании".</span><span class="sxs-lookup"><span data-stu-id="866a3-555">Having a default limit mitigates a potential DoS attack.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-556">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-556">Mitigations</span></span>

<span data-ttu-id="866a3-557">В большинстве случаев вызывать `AddEntityFramework*` не требуется, если также вызывается метод `AddDbContext` или `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="866a3-557">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="866a3-558">Поэтому лучшее решение — удалить вызов `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="866a3-558">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="866a3-559">Если для работы вашего приложения нужны эти службы, заранее зарегистрируйте реализацию IMemoryCache явным образом в контейнере внедрения зависимостей с помощью метода [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="866a3-559">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="866a3-560">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="866a3-560">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="866a3-561">Отслеживание вопроса № 13552</span><span class="sxs-lookup"><span data-stu-id="866a3-561">Tracking Issue #13552</span></span>](https://github.com/dotnet/efcore/issues/13552)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-562">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-562">Old behavior</span></span>

<span data-ttu-id="866a3-563">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="866a3-563">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="866a3-564">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="866a3-564">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-565">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-565">New behavior</span></span>

<span data-ttu-id="866a3-566">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="866a3-566">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="866a3-567">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="866a3-567">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="866a3-568">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="866a3-568">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="866a3-569">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="866a3-569">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-570">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-570">Why</span></span>

<span data-ttu-id="866a3-571">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="866a3-571">This change was made to improve the default performance of using `context.Entry`.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-572">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-572">Mitigations</span></span>

<span data-ttu-id="866a3-573">Вызовите `ChangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="866a3-573">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="866a3-574">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="866a3-574">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="866a3-575">Отслеживание вопроса № 14617</span><span class="sxs-lookup"><span data-stu-id="866a3-575">Tracking Issue #14617</span></span>](https://github.com/dotnet/efcore/issues/14617)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-576">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-576">Old behavior</span></span>

<span data-ttu-id="866a3-577">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="866a3-577">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="866a3-578">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="866a3-578">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-579">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-579">New behavior</span></span>

<span data-ttu-id="866a3-580">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="866a3-580">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-581">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-581">Why</span></span>

<span data-ttu-id="866a3-582">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="866a3-582">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-583">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-583">Mitigations</span></span>

<span data-ttu-id="866a3-584">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="866a3-584">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="866a3-585">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="866a3-585">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="866a3-586">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="866a3-586">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="866a3-587">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="866a3-587">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="866a3-588">Отслеживание вопроса № 14698</span><span class="sxs-lookup"><span data-stu-id="866a3-588">Tracking Issue #14698</span></span>](https://github.com/dotnet/efcore/issues/14698)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-589">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-589">Old behavior</span></span>

<span data-ttu-id="866a3-590">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="866a3-590">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-591">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-591">New behavior</span></span>

<span data-ttu-id="866a3-592">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="866a3-592">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-593">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-593">Why</span></span>

<span data-ttu-id="866a3-594">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="866a3-594">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-595">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-595">Mitigations</span></span>

<span data-ttu-id="866a3-596">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-596">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="866a3-597">Это нетипично.</span><span class="sxs-lookup"><span data-stu-id="866a3-597">This isn't common.</span></span>
<span data-ttu-id="866a3-598">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="866a3-598">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="866a3-599">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/dotnet/efcore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="866a3-599">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/dotnet/efcore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="866a3-600">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="866a3-600">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="866a3-601">Отслеживание вопроса № 12780</span><span class="sxs-lookup"><span data-stu-id="866a3-601">Tracking Issue #12780</span></span>](https://github.com/dotnet/efcore/issues/12780)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-602">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-602">Old behavior</span></span>

<span data-ttu-id="866a3-603">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="866a3-603">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="866a3-604">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="866a3-604">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="866a3-605">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="866a3-605">In these cases, attempting to lazy-load would be a no-op.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-606">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-606">New behavior</span></span>

<span data-ttu-id="866a3-607">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="866a3-607">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="866a3-608">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="866a3-608">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="866a3-609">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="866a3-609">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="866a3-610">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="866a3-610">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-611">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-611">Why</span></span>

<span data-ttu-id="866a3-612">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="866a3-612">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-613">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-613">Mitigations</span></span>

<span data-ttu-id="866a3-614">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="866a3-614">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="866a3-615">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="866a3-615">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="866a3-616">Отслеживание вопроса № 10236</span><span class="sxs-lookup"><span data-stu-id="866a3-616">Tracking Issue #10236</span></span>](https://github.com/dotnet/efcore/issues/10236)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-617">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-617">Old behavior</span></span>

<span data-ttu-id="866a3-618">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="866a3-618">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-619">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-619">New behavior</span></span>

<span data-ttu-id="866a3-620">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="866a3-620">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-621">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-621">Why</span></span>

<span data-ttu-id="866a3-622">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="866a3-622">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-623">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-623">Mitigations</span></span>

<span data-ttu-id="866a3-624">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="866a3-624">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="866a3-625">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="866a3-625">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="866a3-626">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-626">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="866a3-627">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="866a3-627">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="866a3-628">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="866a3-628">Tracking Issue #9171</span></span>](https://github.com/dotnet/efcore/issues/9171)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-629">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-629">Old behavior</span></span>

<span data-ttu-id="866a3-630">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="866a3-630">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="866a3-631">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-631">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="866a3-632">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="866a3-632">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="866a3-633">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="866a3-633">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-634">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-634">New behavior</span></span>

<span data-ttu-id="866a3-635">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="866a3-635">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-636">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-636">Why</span></span>

<span data-ttu-id="866a3-637">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="866a3-637">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-638">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-638">Mitigations</span></span>

<span data-ttu-id="866a3-639">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="866a3-639">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="866a3-640">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="866a3-640">This is not common.</span></span>
<span data-ttu-id="866a3-641">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="866a3-641">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="866a3-642">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-642">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="866a3-643">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="866a3-643">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="866a3-644">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="866a3-644">Tracking Issue #15184</span></span>](https://github.com/dotnet/efcore/issues/15184)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-645">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-645">Old behavior</span></span>

<span data-ttu-id="866a3-646">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="866a3-646">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="866a3-647">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="866a3-647">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-648">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-648">New behavior</span></span>

<span data-ttu-id="866a3-649">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="866a3-649">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-650">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-650">Why</span></span>

<span data-ttu-id="866a3-651">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="866a3-651">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-652">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-652">Mitigations</span></span>

<span data-ttu-id="866a3-653">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="866a3-653">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="866a3-654">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="866a3-654">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="866a3-655">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="866a3-655">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="866a3-656">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="866a3-656">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="866a3-657">Отслеживание вопроса № 9913</span><span class="sxs-lookup"><span data-stu-id="866a3-657">Tracking Issue #9913</span></span>](https://github.com/dotnet/efcore/issues/9913)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-658">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-658">Old behavior</span></span>

<span data-ttu-id="866a3-659">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="866a3-659">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-660">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-660">New behavior</span></span>

<span data-ttu-id="866a3-661">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="866a3-661">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-662">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-662">Why</span></span>

<span data-ttu-id="866a3-663">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="866a3-663">Type mappings are now used for more than just relational database providers.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-664">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-664">Mitigations</span></span>

<span data-ttu-id="866a3-665">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="866a3-665">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="866a3-666">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="866a3-666">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="866a3-667">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="866a3-667">ToTable on a derived type throws an exception</span></span>

[<span data-ttu-id="866a3-668">Отслеживание вопроса № 11811</span><span class="sxs-lookup"><span data-stu-id="866a3-668">Tracking Issue #11811</span></span>](https://github.com/dotnet/efcore/issues/11811)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-669">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-669">Old behavior</span></span>

<span data-ttu-id="866a3-670">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="866a3-670">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-671">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-671">New behavior</span></span>

<span data-ttu-id="866a3-672">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="866a3-672">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-673">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-673">Why</span></span>

<span data-ttu-id="866a3-674">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="866a3-674">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="866a3-675">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="866a3-675">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-676">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-676">Mitigations</span></span>

<span data-ttu-id="866a3-677">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="866a3-677">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="866a3-678">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="866a3-678">ForSqlServerHasIndex replaced with HasIndex</span></span>

[<span data-ttu-id="866a3-679">Отслеживание вопроса № 12366</span><span class="sxs-lookup"><span data-stu-id="866a3-679">Tracking Issue #12366</span></span>](https://github.com/dotnet/efcore/issues/12366)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-680">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-680">Old behavior</span></span>

<span data-ttu-id="866a3-681">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="866a3-681">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-682">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-682">New behavior</span></span>

<span data-ttu-id="866a3-683">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="866a3-683">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="866a3-684">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="866a3-684">Use `HasIndex().ForSqlServerInclude()`.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-685">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-685">Why</span></span>

<span data-ttu-id="866a3-686">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="866a3-686">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-687">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-687">Mitigations</span></span>

<span data-ttu-id="866a3-688">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="866a3-688">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="866a3-689">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="866a3-689">Metadata API changes</span></span>

[<span data-ttu-id="866a3-690">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="866a3-690">Tracking Issue #214</span></span>](https://github.com/dotnet/efcore/issues/214)

#### <a name="new-behavior"></a><span data-ttu-id="866a3-691">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-691">New behavior</span></span>

<span data-ttu-id="866a3-692">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="866a3-692">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

#### <a name="why"></a><span data-ttu-id="866a3-693">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-693">Why</span></span>

<span data-ttu-id="866a3-694">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="866a3-694">This change simplifies the implementation of the aforementioned interfaces.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-695">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-695">Mitigations</span></span>

<span data-ttu-id="866a3-696">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="866a3-696">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="866a3-697">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="866a3-697">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="866a3-698">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="866a3-698">Tracking Issue #214</span></span>](https://github.com/dotnet/efcore/issues/214)

#### <a name="new-behavior"></a><span data-ttu-id="866a3-699">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-699">New behavior</span></span>

<span data-ttu-id="866a3-700">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="866a3-700">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

#### <a name="why"></a><span data-ttu-id="866a3-701">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-701">Why</span></span>

<span data-ttu-id="866a3-702">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="866a3-702">This change simplifies the implementation of the aforementioned extension methods.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-703">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-703">Mitigations</span></span>

<span data-ttu-id="866a3-704">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="866a3-704">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="866a3-705">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="866a3-705">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="866a3-706">Отслеживание вопроса № 12151</span><span class="sxs-lookup"><span data-stu-id="866a3-706">Tracking Issue #12151</span></span>](https://github.com/dotnet/efcore/issues/12151)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-707">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-707">Old behavior</span></span>

<span data-ttu-id="866a3-708">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="866a3-708">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-709">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-709">New behavior</span></span>

<span data-ttu-id="866a3-710">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="866a3-710">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-711">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-711">Why</span></span>

<span data-ttu-id="866a3-712">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="866a3-712">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-713">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-713">Mitigations</span></span>

<span data-ttu-id="866a3-714">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-714">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="866a3-715">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="866a3-715">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="866a3-716">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="866a3-716">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="866a3-717">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-717">Old behavior</span></span>

<span data-ttu-id="866a3-718">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="866a3-718">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-719">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-719">New behavior</span></span>

<span data-ttu-id="866a3-720">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="866a3-720">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-721">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-721">Why</span></span>

<span data-ttu-id="866a3-722">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="866a3-722">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-723">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-723">Mitigations</span></span>

<span data-ttu-id="866a3-724">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="866a3-724">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="866a3-725">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="866a3-725">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="866a3-726">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="866a3-726">Tracking Issue #15078</span></span>](https://github.com/dotnet/efcore/issues/15078)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-727">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-727">Old behavior</span></span>

<span data-ttu-id="866a3-728">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="866a3-728">Guid values were previously stored as BLOB values on SQLite.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-729">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-729">New behavior</span></span>

<span data-ttu-id="866a3-730">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="866a3-730">Guid values are now stored as TEXT.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-731">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-731">Why</span></span>

<span data-ttu-id="866a3-732">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="866a3-732">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="866a3-733">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="866a3-733">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-734">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-734">Mitigations</span></span>

<span data-ttu-id="866a3-735">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="866a3-735">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

```sql
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

<span data-ttu-id="866a3-736">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="866a3-736">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

```csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="866a3-737">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="866a3-737">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="866a3-738">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="866a3-738">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="866a3-739">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="866a3-739">Tracking Issue #15020</span></span>](https://github.com/dotnet/efcore/issues/15020)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-740">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-740">Old behavior</span></span>

<span data-ttu-id="866a3-741">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="866a3-741">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="866a3-742">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="866a3-742">For example, a char value of *A* was stored as the integer value 65.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-743">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-743">New behavior</span></span>

<span data-ttu-id="866a3-744">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="866a3-744">Char values are now stored as TEXT.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-745">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-745">Why</span></span>

<span data-ttu-id="866a3-746">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="866a3-746">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-747">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-747">Mitigations</span></span>

<span data-ttu-id="866a3-748">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="866a3-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

```sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="866a3-749">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="866a3-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

```csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="866a3-750">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="866a3-750">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="866a3-751">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="866a3-751">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="866a3-752">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="866a3-752">Tracking Issue #12978</span></span>](https://github.com/dotnet/efcore/issues/12978)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-753">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-753">Old behavior</span></span>

<span data-ttu-id="866a3-754">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="866a3-754">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-755">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-755">New behavior</span></span>

<span data-ttu-id="866a3-756">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="866a3-756">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-757">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-757">Why</span></span>

<span data-ttu-id="866a3-758">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="866a3-758">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="866a3-759">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="866a3-759">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-760">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-760">Mitigations</span></span>

<span data-ttu-id="866a3-761">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="866a3-761">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="866a3-762">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="866a3-762">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="866a3-763">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="866a3-763">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

```diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="866a3-764">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="866a3-764">The Migrations history table also needs to be updated.</span></span>

```sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="866a3-765">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="866a3-765">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="866a3-766">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="866a3-766">Tracking Issue #16400</span></span>](https://github.com/dotnet/efcore/issues/16400)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-767">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-767">Old behavior</span></span>

<span data-ttu-id="866a3-768">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="866a3-768">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-769">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-769">New behavior</span></span>

<span data-ttu-id="866a3-770">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="866a3-770">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-771">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-771">Why</span></span>

<span data-ttu-id="866a3-772">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="866a3-772">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-773">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-773">Mitigations</span></span>

<span data-ttu-id="866a3-774">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="866a3-774">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="866a3-775">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/dotnet/efcore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="866a3-775">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/dotnet/efcore/issues/16400) with details.</span></span> <span data-ttu-id="866a3-776">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="866a3-776">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="866a3-777">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="866a3-777">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="866a3-778">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="866a3-778">Tracking Issue #16119</span></span>](https://github.com/dotnet/efcore/issues/16119)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-779">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-779">Old behavior</span></span>

<span data-ttu-id="866a3-780">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="866a3-780">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-781">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-781">New behavior</span></span>

<span data-ttu-id="866a3-782">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="866a3-782">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-783">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-783">Why</span></span>

<span data-ttu-id="866a3-784">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="866a3-784">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="866a3-785">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="866a3-785">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-786">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-786">Mitigations</span></span>

<span data-ttu-id="866a3-787">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="866a3-787">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="866a3-788">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-788">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="866a3-789">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="866a3-789">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="866a3-790">Отслеживание вопроса № 10985</span><span class="sxs-lookup"><span data-stu-id="866a3-790">Tracking Issue #10985</span></span>](https://github.com/dotnet/efcore/issues/10985)

#### <a name="change"></a><span data-ttu-id="866a3-791">Change</span><span class="sxs-lookup"><span data-stu-id="866a3-791">Change</span></span>

<span data-ttu-id="866a3-792">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="866a3-792">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-793">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-793">Why</span></span>

<span data-ttu-id="866a3-794">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="866a3-794">Aligns the naming of this warning event with all other warning events.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-795">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-795">Mitigations</span></span>

<span data-ttu-id="866a3-796">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="866a3-796">Use the new name.</span></span> <span data-ttu-id="866a3-797">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="866a3-797">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="866a3-798">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="866a3-798">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="866a3-799">Отслеживание вопроса № 10730</span><span class="sxs-lookup"><span data-stu-id="866a3-799">Tracking Issue #10730</span></span>](https://github.com/dotnet/efcore/issues/10730)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-800">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-800">Old behavior</span></span>

<span data-ttu-id="866a3-801">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="866a3-801">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="866a3-802">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-802">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

#### <a name="new-behavior"></a><span data-ttu-id="866a3-803">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-803">New behavior</span></span>

<span data-ttu-id="866a3-804">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="866a3-804">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="866a3-805">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-805">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

#### <a name="why"></a><span data-ttu-id="866a3-806">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-806">Why</span></span>

<span data-ttu-id="866a3-807">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="866a3-807">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-808">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-808">Mitigations</span></span>

<span data-ttu-id="866a3-809">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="866a3-809">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="866a3-810">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="866a3-810">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="866a3-811">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="866a3-811">Tracking Issue #15997</span></span>](https://github.com/dotnet/efcore/issues/15997)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-812">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-812">Old behavior</span></span>

<span data-ttu-id="866a3-813">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="866a3-813">Before EF Core 3.0, these methods were protected.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-814">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-814">New behavior</span></span>

<span data-ttu-id="866a3-815">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="866a3-815">Starting with EF Core 3.0, these methods are public.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-816">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-816">Why</span></span>

<span data-ttu-id="866a3-817">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="866a3-817">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="866a3-818">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="866a3-818">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-819">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-819">Mitigations</span></span>

<span data-ttu-id="866a3-820">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="866a3-820">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="866a3-821">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="866a3-821">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="866a3-822">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="866a3-822">Tracking Issue #11506</span></span>](https://github.com/dotnet/efcore/issues/11506)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-823">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-823">Old behavior</span></span>

<span data-ttu-id="866a3-824">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="866a3-824">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-825">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-825">New behavior</span></span>

<span data-ttu-id="866a3-826">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="866a3-826">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="866a3-827">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="866a3-827">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-828">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-828">Why</span></span>

<span data-ttu-id="866a3-829">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="866a3-829">This package is only intended to be used at design time.</span></span> <span data-ttu-id="866a3-830">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="866a3-830">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="866a3-831">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="866a3-831">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-832">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-832">Mitigations</span></span>

<span data-ttu-id="866a3-833">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="866a3-833">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="866a3-834">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="866a3-834">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="866a3-835">Такую явную ссылку необходимо добавить в любой проект, где требуются типы из пакета.</span><span class="sxs-lookup"><span data-stu-id="866a3-835">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="866a3-836">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="866a3-836">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="866a3-837">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="866a3-837">Tracking Issue #14824</span></span>](https://github.com/dotnet/efcore/issues/14824)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-838">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-838">Old behavior</span></span>

<span data-ttu-id="866a3-839">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="866a3-839">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-840">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-840">New behavior</span></span>

<span data-ttu-id="866a3-841">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="866a3-841">We've updated our package to depend on version 2.0.0.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-842">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-842">Why</span></span>

<span data-ttu-id="866a3-843">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="866a3-843">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="866a3-844">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="866a3-844">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-845">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-845">Mitigations</span></span>

<span data-ttu-id="866a3-846">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="866a3-846">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="866a3-847">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="866a3-847">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="866a3-848">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="866a3-848">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="866a3-849">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="866a3-849">Tracking Issue #14825</span></span>](https://github.com/dotnet/efcore/issues/14825)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-850">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-850">Old behavior</span></span>

<span data-ttu-id="866a3-851">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="866a3-851">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-852">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-852">New behavior</span></span>

<span data-ttu-id="866a3-853">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="866a3-853">We've update our package to depend on version 2.0.0.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-854">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-854">Why</span></span>

<span data-ttu-id="866a3-855">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="866a3-855">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-856">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-856">Mitigations</span></span>

<span data-ttu-id="866a3-857">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="866a3-857">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="866a3-858">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="866a3-858">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="866a3-859">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="866a3-859">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="866a3-860">Отслеживание вопроса № 15636</span><span class="sxs-lookup"><span data-stu-id="866a3-860">Tracking Issue #15636</span></span>](https://github.com/dotnet/efcore/issues/15636)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-861">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-861">Old behavior</span></span>

<span data-ttu-id="866a3-862">Пакет Microsoft.EntityFrameworkCore.SqlServer ранее зависел от System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="866a3-862">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="866a3-863">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-863">New behavior</span></span>

<span data-ttu-id="866a3-864">Мы обновили пакет, и теперь он зависит от Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="866a3-864">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-865">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-865">Why</span></span>

<span data-ttu-id="866a3-866">Microsoft.Data.SqlClient теперь является основным разрабатываемым драйвером для доступа к SQL Server вместо System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="866a3-866">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="866a3-867">Некоторые важные функции, такие как Always Encrypted, доступны только в Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="866a3-867">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-868">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-868">Mitigations</span></span>

<span data-ttu-id="866a3-869">Если код включает прямую зависимость от System.Data.SqlClient, необходимо изменить код так, чтобы он ссылался на Microsoft.Data.SqlClient. Поскольку API двух пакетов очень хорошо совместимы, достаточно изменить пакет и пространство имен.</span><span class="sxs-lookup"><span data-stu-id="866a3-869">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="866a3-870">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="866a3-870">Multiple ambiguous self-referencing relationships must be configured</span></span>

[<span data-ttu-id="866a3-871">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="866a3-871">Tracking Issue #13573</span></span>](https://github.com/dotnet/efcore/issues/13573)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-872">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-872">Old behavior</span></span>

<span data-ttu-id="866a3-873">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="866a3-873">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="866a3-874">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-874">For example:</span></span>

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

#### <a name="new-behavior"></a><span data-ttu-id="866a3-875">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-875">New behavior</span></span>

<span data-ttu-id="866a3-876">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="866a3-876">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-877">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-877">Why</span></span>

<span data-ttu-id="866a3-878">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="866a3-878">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-879">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-879">Mitigations</span></span>

<span data-ttu-id="866a3-880">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="866a3-880">Use full configuration of the relationship.</span></span> <span data-ttu-id="866a3-881">Пример:</span><span class="sxs-lookup"><span data-stu-id="866a3-881">For example:</span></span>

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

### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="866a3-882">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="866a3-882">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="866a3-883">Отслеживание вопроса № 12757</span><span class="sxs-lookup"><span data-stu-id="866a3-883">Tracking Issue #12757</span></span>](https://github.com/dotnet/efcore/issues/12757)

#### <a name="old-behavior"></a><span data-ttu-id="866a3-884">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-884">Old behavior</span></span>

<span data-ttu-id="866a3-885">DbFunction, настроенная со схемой в виде пустой строки, обрабатывалась как встроенная функция без схемы.</span><span class="sxs-lookup"><span data-stu-id="866a3-885">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="866a3-886">Например, следующий код сопоставит функцию CLR `DatePart` со встроенной функцией `DATEPART` в SqlServer.</span><span class="sxs-lookup"><span data-stu-id="866a3-886">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

#### <a name="new-behavior"></a><span data-ttu-id="866a3-887">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="866a3-887">New behavior</span></span>

<span data-ttu-id="866a3-888">Все сопоставления DbFunction считаются сопоставленными с определяемыми пользователем функциями.</span><span class="sxs-lookup"><span data-stu-id="866a3-888">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="866a3-889">Поэтому пустое строковое значение поместит функцию в схему по умолчанию для модели.</span><span class="sxs-lookup"><span data-stu-id="866a3-889">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="866a3-890">Это может быть схема, настроенная явным образом с помощью `modelBuilder.HasDefaultSchema()` текучего API либо `dbo`.</span><span class="sxs-lookup"><span data-stu-id="866a3-890">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

#### <a name="why"></a><span data-ttu-id="866a3-891">Почему</span><span class="sxs-lookup"><span data-stu-id="866a3-891">Why</span></span>

<span data-ttu-id="866a3-892">Ранее эта функция считалась встроенной из-за пустой схемы, но эта логика применима только к SqlServer, где встроенные функции не принадлежат ни одной схеме.</span><span class="sxs-lookup"><span data-stu-id="866a3-892">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="866a3-893">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="866a3-893">Mitigations</span></span>

<span data-ttu-id="866a3-894">Вручную настройте преобразование функции DbFunction, чтобы сопоставить ее со встроенной функцией.</span><span class="sxs-lookup"><span data-stu-id="866a3-894">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```

<a name="netstandard21"></a>

### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20-reverted"></a><span data-ttu-id="866a3-895">~~Версия EF Core 3.0 больше предназначена для .NET Standard 2.1, а не для .NET Standard 2.0~~ отменено</span><span class="sxs-lookup"><span data-stu-id="866a3-895">~~EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0~~ Reverted</span></span>

[<span data-ttu-id="866a3-896">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="866a3-896">Tracking Issue #15498</span></span>](https://github.com/dotnet/efcore/issues/15498)

<span data-ttu-id="866a3-897">Версия EF Core 3.0 была больше предназначена для .NET Standard 2.1. Это критическое изменение, которое исключало поддержку приложений .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="866a3-897">EF Core 3.0 targets .NET Standard 2.1, which is a breaking change which excludes .NET Framework applications.</span></span> <span data-ttu-id="866a3-898">Это изменение отменено в EF Core 3.1, и EF Core снова ориентируется на .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="866a3-898">EF Core 3.1 reverted this and targets .NET Standard 2.0 again.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="866a3-899">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="866a3-899">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="866a3-900">Отслеживание вопроса № 14523</span><span class="sxs-lookup"><span data-stu-id="866a3-900">Tracking Issue #14523</span></span>](https://github.com/dotnet/efcore/issues/14523)

<span data-ttu-id="866a3-901">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="866a3-901">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="866a3-902">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="866a3-902">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```
