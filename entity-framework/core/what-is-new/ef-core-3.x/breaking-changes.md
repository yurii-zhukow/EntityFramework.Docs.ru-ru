---
title: Критические изменения в EF Core 3.x — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 3.x
author: ajcvickers
ms.date: 09/05/2020
uid: core/what-is-new/ef-core-3.x/breaking-changes
ms.openlocfilehash: 8c0be4193c79e838e40bfc2dc10c9d12b01381cd
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128775"
---
# <a name="breaking-changes-included-in-ef-core-3x"></a><span data-ttu-id="2771e-103">Критические изменения в EF Core 3.x</span><span class="sxs-lookup"><span data-stu-id="2771e-103">Breaking changes included in EF Core 3.x</span></span>

<span data-ttu-id="2771e-104">Указанные ниже изменения API и поведения могут нарушать работу существующих приложений при их обновлении до версии 3.x.</span><span class="sxs-lookup"><span data-stu-id="2771e-104">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.x.</span></span>
<span data-ttu-id="2771e-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="2771e-105">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="2771e-106">Сводка</span><span class="sxs-lookup"><span data-stu-id="2771e-106">Summary</span></span>

| <span data-ttu-id="2771e-107">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="2771e-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2771e-108">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="2771e-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2771e-109">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="2771e-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2771e-110">Высокий</span><span class="sxs-lookup"><span data-stu-id="2771e-110">High</span></span>       |
| [<span data-ttu-id="2771e-111">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="2771e-111">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2771e-112">Высокий</span><span class="sxs-lookup"><span data-stu-id="2771e-112">High</span></span>      |
| [<span data-ttu-id="2771e-113">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="2771e-113">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2771e-114">Высокий</span><span class="sxs-lookup"><span data-stu-id="2771e-114">High</span></span>      |
| [<span data-ttu-id="2771e-115">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="2771e-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2771e-116">Высокий</span><span class="sxs-lookup"><span data-stu-id="2771e-116">High</span></span>      |
| [<span data-ttu-id="2771e-117">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="2771e-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2771e-118">Высокий</span><span class="sxs-lookup"><span data-stu-id="2771e-118">High</span></span>      |
| [<span data-ttu-id="2771e-119">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2771e-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2771e-120">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-120">Medium</span></span>      |
| [<span data-ttu-id="2771e-121">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="2771e-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2771e-122">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-122">Medium</span></span>      |
| [<span data-ttu-id="2771e-123">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="2771e-123">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="2771e-124">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-124">Medium</span></span>      |
| [<span data-ttu-id="2771e-125">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="2771e-125">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2771e-126">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-126">Medium</span></span>      |
| [<span data-ttu-id="2771e-127">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="2771e-127">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2771e-128">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-128">Medium</span></span>      |
| [<span data-ttu-id="2771e-129">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="2771e-129">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2771e-130">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-130">Medium</span></span>      |
| [<span data-ttu-id="2771e-131">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="2771e-131">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="2771e-132">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-132">Medium</span></span>      |
| [<span data-ttu-id="2771e-133">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="2771e-133">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2771e-134">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-134">Medium</span></span>      |
| [<span data-ttu-id="2771e-135">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="2771e-135">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2771e-136">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-136">Medium</span></span>      |
| [<span data-ttu-id="2771e-137">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="2771e-137">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2771e-138">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-138">Medium</span></span>      |
| [<span data-ttu-id="2771e-139">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="2771e-139">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="2771e-140">Средний</span><span class="sxs-lookup"><span data-stu-id="2771e-140">Medium</span></span>      |
| [<span data-ttu-id="2771e-141">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="2771e-141">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2771e-142">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-142">Low</span></span>      |
| [<span data-ttu-id="2771e-143">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="2771e-143">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2771e-144">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-144">Low</span></span>      |
| [<span data-ttu-id="2771e-145">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="2771e-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2771e-146">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-146">Low</span></span>      |
| [<span data-ttu-id="2771e-147">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="2771e-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2771e-148">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-148">Low</span></span>      |
| [<span data-ttu-id="2771e-149">Собственные сущности нельзя запрашивать без использования запроса отслеживания владельцем</span><span class="sxs-lookup"><span data-stu-id="2771e-149">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="2771e-150">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-150">Low</span></span>      |
| [<span data-ttu-id="2771e-151">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="2771e-151">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2771e-152">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-152">Low</span></span>      |
| [<span data-ttu-id="2771e-153">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="2771e-153">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2771e-154">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-154">Low</span></span>      |
| [<span data-ttu-id="2771e-155">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2771e-155">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2771e-156">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-156">Low</span></span>      |
| [<span data-ttu-id="2771e-157">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="2771e-157">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2771e-158">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-158">Low</span></span>      |
| [<span data-ttu-id="2771e-159">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="2771e-159">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2771e-160">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-160">Low</span></span>      |
| [<span data-ttu-id="2771e-161">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="2771e-161">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2771e-162">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-162">Low</span></span>      |
| [<span data-ttu-id="2771e-163">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2771e-163">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2771e-164">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-164">Low</span></span>      |
| [<span data-ttu-id="2771e-165">AddEntityFramework\* добавляет IMemoryCache с ограниченным размером</span><span class="sxs-lookup"><span data-stu-id="2771e-165">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="2771e-166">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-166">Low</span></span>      |
| [<span data-ttu-id="2771e-167">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="2771e-167">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2771e-168">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-168">Low</span></span>      |
| [<span data-ttu-id="2771e-169">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2771e-169">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2771e-170">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-170">Low</span></span>      |
| [<span data-ttu-id="2771e-171">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="2771e-171">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2771e-172">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-172">Low</span></span>      |
| [<span data-ttu-id="2771e-173">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="2771e-173">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2771e-174">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-174">Low</span></span>      |
| [<span data-ttu-id="2771e-175">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2771e-175">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2771e-176">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-176">Low</span></span>      |
| [<span data-ttu-id="2771e-177">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="2771e-177">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2771e-178">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-178">Low</span></span>      |
| [<span data-ttu-id="2771e-179">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="2771e-179">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2771e-180">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-180">Low</span></span>      |
| [<span data-ttu-id="2771e-181">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2771e-181">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2771e-182">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-182">Low</span></span>      |
| [<span data-ttu-id="2771e-183">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="2771e-183">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2771e-184">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-184">Low</span></span>      |
| [<span data-ttu-id="2771e-185">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="2771e-185">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2771e-186">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-186">Low</span></span>      |
| [<span data-ttu-id="2771e-187">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2771e-187">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2771e-188">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-188">Low</span></span>      |
| [<span data-ttu-id="2771e-189">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="2771e-189">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2771e-190">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-190">Low</span></span>      |
| [<span data-ttu-id="2771e-191">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="2771e-191">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2771e-192">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-192">Low</span></span>      |
| [<span data-ttu-id="2771e-193">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="2771e-193">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2771e-194">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-194">Low</span></span>      |
| [<span data-ttu-id="2771e-195">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="2771e-195">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2771e-196">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-196">Low</span></span>      |
| [<span data-ttu-id="2771e-197">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="2771e-197">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2771e-198">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-198">Low</span></span>      |
| [<span data-ttu-id="2771e-199">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="2771e-199">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2771e-200">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-200">Low</span></span>      |
| [<span data-ttu-id="2771e-201">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="2771e-201">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2771e-202">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-202">Low</span></span>      |
| [<span data-ttu-id="2771e-203">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2771e-203">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2771e-204">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-204">Low</span></span>      |
| [<span data-ttu-id="2771e-205">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2771e-205">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2771e-206">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-206">Low</span></span>      |
| [<span data-ttu-id="2771e-207">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2771e-207">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2771e-208">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-208">Low</span></span>      |
| [<span data-ttu-id="2771e-209">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="2771e-209">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="2771e-210">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-210">Low</span></span>      |
| [<span data-ttu-id="2771e-211">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="2771e-211">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2771e-212">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-212">Low</span></span>      |
| [<span data-ttu-id="2771e-213">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="2771e-213">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="2771e-214">Низкий</span><span class="sxs-lookup"><span data-stu-id="2771e-214">Low</span></span>      |
| [<span data-ttu-id="2771e-215">~~Версия EF Core 3.0 больше предназначена для .NET Standard 2.1, а не для .NET Standard 2.0~~ отменено</span><span class="sxs-lookup"><span data-stu-id="2771e-215">~~EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0~~ Reverted</span></span>](#netstandard21) | |
| [<span data-ttu-id="2771e-216">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="2771e-216">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | |

## <a name="high-impact-changes"></a><span data-ttu-id="2771e-217">Изменения высокой степени влияния</span><span class="sxs-lookup"><span data-stu-id="2771e-217">High-impact changes</span></span>

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2771e-218">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="2771e-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2771e-219">[Отслеживание вопроса № 14935](https://github.com/dotnet/efcore/issues/14935)
[Также см. вопрос № 12795](https://github.com/dotnet/efcore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2771e-219">[Tracking Issue #14935](https://github.com/dotnet/efcore/issues/14935)
[Also see issue #12795](https://github.com/dotnet/efcore/issues/12795)</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2771e-220">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-220">Old behavior</span></span>

<span data-ttu-id="2771e-221">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="2771e-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2771e-222">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="2771e-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-223">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-223">New behavior</span></span>

<span data-ttu-id="2771e-224">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="2771e-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2771e-225">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="2771e-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-226">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-226">Why</span></span>

<span data-ttu-id="2771e-227">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="2771e-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2771e-228">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2771e-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2771e-229">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="2771e-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2771e-230">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="2771e-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2771e-231">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="2771e-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2771e-232">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="2771e-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-233">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-233">Mitigations</span></span>

<span data-ttu-id="2771e-234">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="2771e-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>

## <a name="medium-impact-changes"></a><span data-ttu-id="2771e-235">Изменения средней степени влияния</span><span class="sxs-lookup"><span data-stu-id="2771e-235">Medium-impact changes</span></span>

### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2771e-236">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2771e-236">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2771e-237">Отслеживание объявлений о вопросе № 325</span><span class="sxs-lookup"><span data-stu-id="2771e-237">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-238">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-238">Old behavior</span></span>

<span data-ttu-id="2771e-239">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2771e-239">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-240">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-240">New behavior</span></span>

<span data-ttu-id="2771e-241">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-241">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-242">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-242">Why</span></span>

<span data-ttu-id="2771e-243">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2771e-243">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span>
<span data-ttu-id="2771e-244">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="2771e-244">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2771e-245">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="2771e-245">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2771e-246">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-246">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-247">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-247">Mitigations</span></span>

<span data-ttu-id="2771e-248">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="2771e-248">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>

### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2771e-249">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="2771e-249">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2771e-250">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="2771e-250">Tracking Issue #14016</span></span>](https://github.com/dotnet/efcore/issues/14016)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-251">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-251">Old behavior</span></span>

<span data-ttu-id="2771e-252">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="2771e-252">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-253">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-253">New behavior</span></span>

<span data-ttu-id="2771e-254">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="2771e-254">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-255">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-255">Why</span></span>

<span data-ttu-id="2771e-256">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="2771e-256">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-257">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-257">Mitigations</span></span>

<span data-ttu-id="2771e-258">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="2771e-258">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="2771e-259">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](/dotnet/core/tools/global-tools#install-a-local-tool).</span><span class="sxs-lookup"><span data-stu-id="2771e-259">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](/dotnet/core/tools/global-tools#install-a-local-tool).</span></span>

## <a name="low-impact-changes"></a><span data-ttu-id="2771e-260">Изменения низкой степени влияния</span><span class="sxs-lookup"><span data-stu-id="2771e-260">Low-impact changes</span></span>

<a name="fromsql"></a>

### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2771e-261">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="2771e-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2771e-262">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="2771e-262">Tracking Issue #10996</span></span>](https://github.com/dotnet/efcore/issues/10996)

> [!IMPORTANT]
> <span data-ttu-id="2771e-263">`ExecuteSqlCommand` и `ExecuteSqlCommandAsync` являются устаревшими.</span><span class="sxs-lookup"><span data-stu-id="2771e-263">`ExecuteSqlCommand` and `ExecuteSqlCommandAsync` are deprecated.</span></span> <span data-ttu-id="2771e-264">Используйте эти методы вместо них.</span><span class="sxs-lookup"><span data-stu-id="2771e-264">Use these methods instead.</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2771e-265">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-265">Old behavior</span></span>

<span data-ttu-id="2771e-266">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="2771e-266">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-267">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-267">New behavior</span></span>

<span data-ttu-id="2771e-268">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="2771e-268">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2771e-269">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-269">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2771e-270">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="2771e-270">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2771e-271">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-271">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2771e-272">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="2771e-272">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-273">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-273">Why</span></span>

<span data-ttu-id="2771e-274">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="2771e-274">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2771e-275">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="2771e-275">This could result in queries not being parameterized when they should have been.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-276">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-276">Mitigations</span></span>

<span data-ttu-id="2771e-277">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="2771e-277">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>

### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="2771e-278">Метод FromSql не поддерживает составление при использовании с хранимой процедурой</span><span class="sxs-lookup"><span data-stu-id="2771e-278">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="2771e-279">Отслеживание вопроса № 15392</span><span class="sxs-lookup"><span data-stu-id="2771e-279">Tracking Issue #15392</span></span>](https://github.com/dotnet/efcore/issues/15392)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-280">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-280">Old behavior</span></span>

<span data-ttu-id="2771e-281">До версии EF Core 3.0 метод FromSql пытался определить, можно ли выполнить составление для переданного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="2771e-281">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="2771e-282">Если SQL-запрос не допускал составления, как в случае с хранимой процедурой, вычисление производилось на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2771e-282">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="2771e-283">В случае с приведенным ниже запросом хранимая процедура выполнялась на сервере, а метод FirstOrDefault — на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2771e-283">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

#### <a name="new-behavior"></a><span data-ttu-id="2771e-284">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-284">New behavior</span></span>

<span data-ttu-id="2771e-285">Начиная с версии 3.0 платформа EF Core не пытается анализировать код SQL.</span><span class="sxs-lookup"><span data-stu-id="2771e-285">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="2771e-286">Поэтому если вы выполняете составление после метода FromSqlRaw или FromSqlInterpolated, EF Core составляет SQL-запрос, вызывая вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="2771e-286">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="2771e-287">При использовании хранимой процедуры с составлением вы получите исключение, вызванное недопустимым синтаксисом SQL.</span><span class="sxs-lookup"><span data-stu-id="2771e-287">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-288">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-288">Why</span></span>

<span data-ttu-id="2771e-289">EF Core 3.0 не поддерживает автоматическое вычисление на стороне клиента, так как оно было подвержено ошибкам, как описано [здесь](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="2771e-289">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-290">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-290">Mitigations</span></span>

<span data-ttu-id="2771e-291">Если вы используете хранимую процедуру в методе FromSqlRaw или FromSqlInterpolated, то знаете, что составление для нее невозможно, поэтому вы можете добавить __AsEnumerable или AsAsyncEnumerable__ сразу после вызова метода FromSql, чтобы избежать составления на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="2771e-291">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2771e-292">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="2771e-292">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2771e-293">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="2771e-293">Tracking Issue #15704</span></span>](https://github.com/dotnet/efcore/issues/15704)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-294">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-294">Old behavior</span></span>

<span data-ttu-id="2771e-295">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="2771e-295">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-296">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-296">New behavior</span></span>

<span data-ttu-id="2771e-297">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2771e-297">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2771e-298">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="2771e-298">Attempting to specify them anywhere else will result in a compilation error.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-299">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-299">Why</span></span>

<span data-ttu-id="2771e-300">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="2771e-300">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-301">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-301">Mitigations</span></span>

<span data-ttu-id="2771e-302">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="2771e-302">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>

### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="2771e-303">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="2771e-303">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="2771e-304">Отслеживание вопроса № 13518</span><span class="sxs-lookup"><span data-stu-id="2771e-304">Tracking Issue #13518</span></span>](https://github.com/dotnet/efcore/issues/13518)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-305">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-305">Old behavior</span></span>

<span data-ttu-id="2771e-306">До EF Core 3.0 один и тот же экземпляр сущности будет использоваться для каждого вхождения сущности с заданным типом и ИД.</span><span class="sxs-lookup"><span data-stu-id="2771e-306">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="2771e-307">Это соответствует поведению запросов отслеживания.</span><span class="sxs-lookup"><span data-stu-id="2771e-307">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="2771e-308">Например, следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="2771e-308">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```

<span data-ttu-id="2771e-309">возвращает тот же экземпляр `Category` для каждого `Product`, связанного с заданной категорией.</span><span class="sxs-lookup"><span data-stu-id="2771e-309">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-310">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-310">New behavior</span></span>

<span data-ttu-id="2771e-311">Начиная с EF Core 3.0, при обнаружении сущности с заданным типом и ИД в разных местах возвращенного графа будут создаваться различные экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="2771e-311">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="2771e-312">Например, вышеприведенный запрос теперь будет возвращать новый экземпляр `Category` для каждого `Product`, даже если два продукта связаны с одной категорией.</span><span class="sxs-lookup"><span data-stu-id="2771e-312">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-313">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-313">Why</span></span>

<span data-ttu-id="2771e-314">Разрешение идентификаторов (то есть определение того, что сущность имеет тот же тип и идентификатор, что и обнаруженная ранее сущность) добавляет дополнительную нагрузку на производительность и память.</span><span class="sxs-lookup"><span data-stu-id="2771e-314">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="2771e-315">Обычно это идет вразрез с причинами использования запросов без отслеживания.</span><span class="sxs-lookup"><span data-stu-id="2771e-315">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="2771e-316">Кроме того, хотя разрешение идентификаторов иногда может быть полезным, оно не требуется, если сущности должны быть сериализованы и отправлены клиенту, что является распространенным случаем для неотслеживаемых запросов.</span><span class="sxs-lookup"><span data-stu-id="2771e-316">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-317">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-317">Mitigations</span></span>

<span data-ttu-id="2771e-318">Используйте запрос с отслеживанием, если требуется разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="2771e-318">Use a tracking query if identity resolution is required.</span></span>

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2771e-319">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="2771e-319">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2771e-320">Отслеживание вопроса № 12378</span><span class="sxs-lookup"><span data-stu-id="2771e-320">Tracking Issue #12378</span></span>](https://github.com/dotnet/efcore/issues/12378)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-321">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-321">Old behavior</span></span>

<span data-ttu-id="2771e-322">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="2771e-322">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2771e-323">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="2771e-323">Usually these temporary values were large negative numbers.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-324">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-324">New behavior</span></span>

<span data-ttu-id="2771e-325">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="2771e-325">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-326">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-326">Why</span></span>

<span data-ttu-id="2771e-327">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2771e-327">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-328">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-328">Mitigations</span></span>

<span data-ttu-id="2771e-329">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="2771e-329">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2771e-330">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2771e-330">This can be avoided by:</span></span>

* <span data-ttu-id="2771e-331">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="2771e-331">Not using store-generated keys.</span></span>
* <span data-ttu-id="2771e-332">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="2771e-332">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2771e-333">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="2771e-333">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2771e-334">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="2771e-334">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2771e-335">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="2771e-335">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2771e-336">Отслеживание вопроса № 14616</span><span class="sxs-lookup"><span data-stu-id="2771e-336">Tracking Issue #14616</span></span>](https://github.com/dotnet/efcore/issues/14616)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-337">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-337">Old behavior</span></span>

<span data-ttu-id="2771e-338">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2771e-338">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-339">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-339">New behavior</span></span>

<span data-ttu-id="2771e-340">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="2771e-340">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2771e-341">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2771e-341">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2771e-342">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="2771e-342">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-343">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-343">Why</span></span>

<span data-ttu-id="2771e-344">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="2771e-344">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-345">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-345">Mitigations</span></span>

<span data-ttu-id="2771e-346">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="2771e-346">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2771e-347">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="2771e-347">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2771e-348">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="2771e-348">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2771e-349">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="2771e-349">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

<a name="cascade"></a>

### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2771e-350">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="2771e-350">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2771e-351">Отслеживание вопроса № 10114</span><span class="sxs-lookup"><span data-stu-id="2771e-351">Tracking Issue #10114</span></span>](https://github.com/dotnet/efcore/issues/10114)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-352">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-352">Old behavior</span></span>

<span data-ttu-id="2771e-353">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2771e-353">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-354">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-354">New behavior</span></span>

<span data-ttu-id="2771e-355">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="2771e-355">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2771e-356">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="2771e-356">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-357">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-357">Why</span></span>

<span data-ttu-id="2771e-358">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед вызовом_  `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2771e-358">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-359">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-359">Mitigations</span></span>

<span data-ttu-id="2771e-360">Для восстановления прежнего поведения можно использовать параметры `context.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="2771e-360">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="2771e-361">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-361">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

<a name="eager-loading-single-query"></a>

### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="2771e-362">Безотложная загрузка связанных сущностей теперь происходит в одном запросе</span><span class="sxs-lookup"><span data-stu-id="2771e-362">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="2771e-363">Отслеживание вопроса № 18022</span><span class="sxs-lookup"><span data-stu-id="2771e-363">Tracking issue #18022</span></span>](https://github.com/dotnet/efcore/issues/18022)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-364">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-364">Old behavior</span></span>

<span data-ttu-id="2771e-365">До версии 3.0 безотложная загрузка навигаций по коллекциям с помощью операторов `Include` приводила к созданию нескольких запросов к реляционной базе данных, по одной для каждого связанного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="2771e-365">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-366">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-366">New behavior</span></span>

<span data-ttu-id="2771e-367">Начиная с версии 3.0 платформа EF Core создает один запрос с соединениями к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="2771e-367">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-368">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-368">Why</span></span>

<span data-ttu-id="2771e-369">Отправка нескольких запросов для реализации одного запроса LINQ приводила ко множеству проблем, включая снижение производительности из-за необходимости нескольких циклов обращения к базе данных и проблем с согласованностью данных, так как при каждом запросе состояние базы данных могло быть разным.</span><span class="sxs-lookup"><span data-stu-id="2771e-369">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-370">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-370">Mitigations</span></span>

<span data-ttu-id="2771e-371">Хотя технически это изменение не является критическим, оно может существенно повлиять на производительность приложения в случае, если один запрос содержит большое число операторов `Include` с навигациями по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="2771e-371">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="2771e-372">Дополнительные сведения и указания по повышению эффективности запросов см. в [этом комментарии](https://github.com/dotnet/efcore/issues/18022#issuecomment-542397085).</span><span class="sxs-lookup"><span data-stu-id="2771e-372">[See this comment](https://github.com/dotnet/efcore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>

### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2771e-373">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="2771e-373">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2771e-374">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="2771e-374">Tracking Issue #12661</span></span>](https://github.com/dotnet/efcore/issues/12661)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-375">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-375">Old behavior</span></span>

<span data-ttu-id="2771e-376">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="2771e-376">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-377">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-377">New behavior</span></span>

<span data-ttu-id="2771e-378">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="2771e-378">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-379">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-379">Why</span></span>

<span data-ttu-id="2771e-380">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="2771e-380">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-381">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-381">Mitigations</span></span>

<span data-ttu-id="2771e-382">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="2771e-382">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>

### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2771e-383">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="2771e-383">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2771e-384">Отслеживание вопроса № 14194</span><span class="sxs-lookup"><span data-stu-id="2771e-384">Tracking Issue #14194</span></span>](https://github.com/dotnet/efcore/issues/14194)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-385">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-385">Old behavior</span></span>

<span data-ttu-id="2771e-386">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/keyless-entity-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="2771e-386">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2771e-387">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="2771e-387">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-388">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-388">New behavior</span></span>

<span data-ttu-id="2771e-389">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="2771e-389">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2771e-390">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="2771e-390">Keyless entity types have the same functionality as query types in previous versions.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-391">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-391">Why</span></span>

<span data-ttu-id="2771e-392">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="2771e-392">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2771e-393">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="2771e-393">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2771e-394">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="2771e-394">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-395">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-395">Mitigations</span></span>

<span data-ttu-id="2771e-396">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="2771e-396">The following parts of the API are now obsolete:</span></span>

* <span data-ttu-id="2771e-397">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="2771e-397">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2771e-398">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="2771e-398">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2771e-399">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2771e-399">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2771e-400">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="2771e-400">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>
* <span data-ttu-id="2771e-401">**`IQueryTypeConfiguration<TQuery>`** — вместо него следует использовать `IEntityTypeConfiguration<TEntity>`.</span><span class="sxs-lookup"><span data-stu-id="2771e-401">**`IQueryTypeConfiguration<TQuery>`** - Instead `IEntityTypeConfiguration<TEntity>` should be used.</span></span>

> [!NOTE]
> <span data-ttu-id="2771e-402">Из-за [проблемы в версиях 3.x](https://github.com/dotnet/efcore/issues/19537) при запросе сущностей без ключей, все свойства которых имеют значение `null`, возвращается `null` вместо сущности. Если эта проблема затрагивает ваш сценарий, также добавьте логику для обработки `null` в результатах.</span><span class="sxs-lookup"><span data-stu-id="2771e-402">Due to [an issue in 3.x](https://github.com/dotnet/efcore/issues/19537) when querying keyless entities that have all properties set to `null` a `null` will be returned instead of an entity, if this issue is applicable to your scenario also add logic to handle `null` in results.</span></span>

<a name="config"></a>

### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2771e-403">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="2771e-403">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2771e-404">[Отслеживание вопроса 12444](https://github.com/dotnet/efcore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/dotnet/efcore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/dotnet/efcore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2771e-404">[Tracking Issue #12444](https://github.com/dotnet/efcore/issues/12444)
[Tracking Issue #9148](https://github.com/dotnet/efcore/issues/9148)
[Tracking Issue #14153](https://github.com/dotnet/efcore/issues/14153)</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2771e-405">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-405">Old behavior</span></span>

<span data-ttu-id="2771e-406">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="2771e-406">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-407">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-407">New behavior</span></span>

<span data-ttu-id="2771e-408">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="2771e-408">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2771e-409">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-409">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2771e-410">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="2771e-410">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2771e-411">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="2771e-411">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2771e-412">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-412">For example:</span></span>

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

<span data-ttu-id="2771e-413">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="2771e-413">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-414">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-414">Why</span></span>

<span data-ttu-id="2771e-415">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="2771e-415">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2771e-416">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="2771e-416">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-417">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-417">Mitigations</span></span>

<span data-ttu-id="2771e-418">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="2771e-418">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2771e-419">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="2771e-419">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2771e-420">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="2771e-420">Tracking Issue #9005</span></span>](https://github.com/dotnet/efcore/issues/9005)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-421">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-421">Old behavior</span></span>

<span data-ttu-id="2771e-422">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="2771e-422">Consider the following model:</span></span>

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

<span data-ttu-id="2771e-423">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2771e-423">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-424">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-424">New behavior</span></span>

<span data-ttu-id="2771e-425">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="2771e-425">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2771e-426">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="2771e-426">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-427">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-427">Mitigations</span></span>

<span data-ttu-id="2771e-428">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="2771e-428">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2771e-429">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="2771e-429">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2771e-430">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="2771e-430">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2771e-431">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="2771e-431">Tracking Issue #14154</span></span>](https://github.com/dotnet/efcore/issues/14154)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-432">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-432">Old behavior</span></span>

<span data-ttu-id="2771e-433">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="2771e-433">Consider the following model:</span></span>

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

<span data-ttu-id="2771e-434">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="2771e-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-435">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-435">New behavior</span></span>

<span data-ttu-id="2771e-436">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2771e-436">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2771e-437">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="2771e-437">Otherwise an exception is thrown during model validation.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-438">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-438">Why</span></span>

<span data-ttu-id="2771e-439">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="2771e-439">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-440">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-440">Mitigations</span></span>

<span data-ttu-id="2771e-441">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="2771e-441">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2771e-442">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="2771e-442">It's possible the create one in shadow-state:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="2771e-443">Собственные сущности нельзя запрашивать без использования запроса отслеживания владельцем</span><span class="sxs-lookup"><span data-stu-id="2771e-443">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="2771e-444">Отслеживание вопроса № 18876</span><span class="sxs-lookup"><span data-stu-id="2771e-444">Tracking Issue #18876</span></span>](https://github.com/dotnet/efcore/issues/18876)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-445">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-445">Old behavior</span></span>

<span data-ttu-id="2771e-446">До версии EF Core 3.0 собственные сущности можно было запрашивать как любую другую навигацию.</span><span class="sxs-lookup"><span data-stu-id="2771e-446">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

#### <a name="new-behavior"></a><span data-ttu-id="2771e-447">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-447">New behavior</span></span>

<span data-ttu-id="2771e-448">Начиная с версии 3.0 EF Core выдает исключение, если запрос отслеживания проецирует собственную сущность без владельца.</span><span class="sxs-lookup"><span data-stu-id="2771e-448">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-449">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-449">Why</span></span>

<span data-ttu-id="2771e-450">Выполнять операции с собственными сущностями без владельца невозможно, поэтому в подавляющем большинстве случаев подобные запросы к ним являются ошибкой.</span><span class="sxs-lookup"><span data-stu-id="2771e-450">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-451">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-451">Mitigations</span></span>

<span data-ttu-id="2771e-452">Если собственную сущность следует отслеживать для изменения в последующем, владелец должен быть включен в запрос.</span><span class="sxs-lookup"><span data-stu-id="2771e-452">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="2771e-453">В противном случае добавьте вызов `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="2771e-453">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2771e-454">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="2771e-454">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2771e-455">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="2771e-455">Tracking Issue #13998</span></span>](https://github.com/dotnet/efcore/issues/13998)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-456">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-456">Old behavior</span></span>

<span data-ttu-id="2771e-457">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="2771e-457">Consider the following model:</span></span>

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

<span data-ttu-id="2771e-458">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2771e-458">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-459">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-459">New behavior</span></span>

<span data-ttu-id="2771e-460">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="2771e-460">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-461">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-461">Why</span></span>

<span data-ttu-id="2771e-462">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="2771e-462">The old behavoir was unexpected.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-463">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-463">Mitigations</span></span>

<span data-ttu-id="2771e-464">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="2771e-464">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2771e-465">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="2771e-465">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2771e-466">Отслеживание вопроса № 13274</span><span class="sxs-lookup"><span data-stu-id="2771e-466">Tracking Issue #13274</span></span>](https://github.com/dotnet/efcore/issues/13274)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-467">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-467">Old behavior</span></span>

<span data-ttu-id="2771e-468">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="2771e-468">Consider the following model:</span></span>

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

<span data-ttu-id="2771e-469">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="2771e-469">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2771e-470">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="2771e-470">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-471">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-471">New behavior</span></span>

<span data-ttu-id="2771e-472">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="2771e-472">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2771e-473">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="2771e-473">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2771e-474">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-474">For example:</span></span>

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

#### <a name="why"></a><span data-ttu-id="2771e-475">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-475">Why</span></span>

<span data-ttu-id="2771e-476">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="2771e-476">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-477">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-477">Mitigations</span></span>

<span data-ttu-id="2771e-478">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="2771e-478">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2771e-479">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2771e-479">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2771e-480">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="2771e-480">Tracking Issue #14218</span></span>](https://github.com/dotnet/efcore/issues/14218)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-481">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-481">Old behavior</span></span>

<span data-ttu-id="2771e-482">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2771e-482">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

#### <a name="new-behavior"></a><span data-ttu-id="2771e-483">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-483">New behavior</span></span>

<span data-ttu-id="2771e-484">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="2771e-484">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-485">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-485">Why</span></span>

<span data-ttu-id="2771e-486">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2771e-486">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2771e-487">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="2771e-487">The new behavior also matches EF6.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-488">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-488">Mitigations</span></span>

<span data-ttu-id="2771e-489">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="2771e-489">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2771e-490">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="2771e-490">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2771e-491">Отслеживание вопроса № 6872</span><span class="sxs-lookup"><span data-stu-id="2771e-491">Tracking Issue #6872</span></span>](https://github.com/dotnet/efcore/issues/6872)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-492">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-492">Old behavior</span></span>

<span data-ttu-id="2771e-493">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="2771e-493">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-494">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-494">New behavior</span></span>

<span data-ttu-id="2771e-495">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="2771e-495">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2771e-496">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="2771e-496">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-497">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-497">Why</span></span>

<span data-ttu-id="2771e-498">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="2771e-498">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-499">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-499">Mitigations</span></span>

<span data-ttu-id="2771e-500">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="2771e-500">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2771e-501">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="2771e-501">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2771e-502">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="2771e-502">Backing fields are used by default</span></span>

[<span data-ttu-id="2771e-503">Отслеживание вопроса № 12430</span><span class="sxs-lookup"><span data-stu-id="2771e-503">Tracking Issue #12430</span></span>](https://github.com/dotnet/efcore/issues/12430)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-504">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-504">Old behavior</span></span>

<span data-ttu-id="2771e-505">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="2771e-505">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2771e-506">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="2771e-506">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-507">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-507">New behavior</span></span>

<span data-ttu-id="2771e-508">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="2771e-508">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2771e-509">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="2771e-509">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-510">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-510">Why</span></span>

<span data-ttu-id="2771e-511">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="2771e-511">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-512">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-512">Mitigations</span></span>

<span data-ttu-id="2771e-513">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2771e-513">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2771e-514">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-514">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2771e-515">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="2771e-515">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2771e-516">Отслеживание вопроса № 12523</span><span class="sxs-lookup"><span data-stu-id="2771e-516">Tracking Issue #12523</span></span>](https://github.com/dotnet/efcore/issues/12523)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-517">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-517">Old behavior</span></span>

<span data-ttu-id="2771e-518">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="2771e-518">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2771e-519">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="2771e-519">This could cause the wrong field to be used in ambiguous cases.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-520">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-520">New behavior</span></span>

<span data-ttu-id="2771e-521">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="2771e-521">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-522">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-522">Why</span></span>

<span data-ttu-id="2771e-523">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="2771e-523">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-524">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-524">Mitigations</span></span>

<span data-ttu-id="2771e-525">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="2771e-525">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2771e-526">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="2771e-526">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2771e-527">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="2771e-527">Field-only property names should match the field name</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2771e-528">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-528">Old behavior</span></span>

<span data-ttu-id="2771e-529">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе .NET, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="2771e-529">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

#### <a name="new-behavior"></a><span data-ttu-id="2771e-530">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-530">New behavior</span></span>

<span data-ttu-id="2771e-531">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="2771e-531">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

#### <a name="why"></a><span data-ttu-id="2771e-532">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-532">Why</span></span>

<span data-ttu-id="2771e-533">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="2771e-533">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-534">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-534">Mitigations</span></span>

<span data-ttu-id="2771e-535">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="2771e-535">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2771e-536">В будущем выпуске EF Core после версии 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства (см. вопрос [№ 15307](https://github.com/dotnet/efcore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="2771e-536">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/dotnet/efcore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2771e-537">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2771e-537">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2771e-538">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="2771e-538">Tracking Issue #14756</span></span>](https://github.com/dotnet/efcore/issues/14756)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-539">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-539">Old behavior</span></span>

<span data-ttu-id="2771e-540">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов методов [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2771e-540">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-541">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-541">New behavior</span></span>

<span data-ttu-id="2771e-542">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="2771e-542">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-543">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-543">Why</span></span>

<span data-ttu-id="2771e-544">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="2771e-544">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2771e-545">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-545">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-546">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-546">Mitigations</span></span>

<span data-ttu-id="2771e-547">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2771e-547">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="2771e-548">AddEntityFramework\* добавляет IMemoryCache с ограниченным размером</span><span class="sxs-lookup"><span data-stu-id="2771e-548">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="2771e-549">Отслеживание вопроса № 12905</span><span class="sxs-lookup"><span data-stu-id="2771e-549">Tracking Issue #12905</span></span>](https://github.com/dotnet/efcore/issues/12905)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-550">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-550">Old behavior</span></span>

<span data-ttu-id="2771e-551">До EF Core 3.0 вызов методов `AddEntityFramework*` также приводил к регистрации служб кэширования памяти с внедрением зависимостей без ограничения размера.</span><span class="sxs-lookup"><span data-stu-id="2771e-551">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-552">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-552">New behavior</span></span>

<span data-ttu-id="2771e-553">Начиная с версии EF Core 3.0 вызов `AddEntityFramework*` приводит к регистрации службы IMemoryCache с ограничением размера.</span><span class="sxs-lookup"><span data-stu-id="2771e-553">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="2771e-554">Если добавленные позже службы зависят от IMemoryCache, они могут быстро достичь ограничения по умолчанию, что приведет к созданию исключений или снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="2771e-554">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-555">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-555">Why</span></span>

<span data-ttu-id="2771e-556">Использование IMemoryCache без ограничения может привести к неконтролируемому использованию памяти, если в логике кэширования запросов есть ошибка или запросы создаются динамически.</span><span class="sxs-lookup"><span data-stu-id="2771e-556">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="2771e-557">Ограничение по умолчанию позволяет предотвращать возможные атаки типа "отказ в обслуживании".</span><span class="sxs-lookup"><span data-stu-id="2771e-557">Having a default limit mitigates a potential DoS attack.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-558">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-558">Mitigations</span></span>

<span data-ttu-id="2771e-559">В большинстве случаев вызывать `AddEntityFramework*` не требуется, если также вызывается метод `AddDbContext` или `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="2771e-559">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="2771e-560">Поэтому лучшее решение — удалить вызов `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="2771e-560">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="2771e-561">Если для работы вашего приложения нужны эти службы, заранее зарегистрируйте реализацию IMemoryCache явным образом в контейнере внедрения зависимостей с помощью метода [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2771e-561">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2771e-562">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="2771e-562">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2771e-563">Отслеживание вопроса № 13552</span><span class="sxs-lookup"><span data-stu-id="2771e-563">Tracking Issue #13552</span></span>](https://github.com/dotnet/efcore/issues/13552)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-564">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-564">Old behavior</span></span>

<span data-ttu-id="2771e-565">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="2771e-565">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2771e-566">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="2771e-566">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-567">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-567">New behavior</span></span>

<span data-ttu-id="2771e-568">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="2771e-568">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2771e-569">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="2771e-569">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2771e-570">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="2771e-570">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2771e-571">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="2771e-571">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-572">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-572">Why</span></span>

<span data-ttu-id="2771e-573">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="2771e-573">This change was made to improve the default performance of using `context.Entry`.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-574">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-574">Mitigations</span></span>

<span data-ttu-id="2771e-575">Вызовите `ChangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="2771e-575">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2771e-576">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2771e-576">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2771e-577">Отслеживание вопроса № 14617</span><span class="sxs-lookup"><span data-stu-id="2771e-577">Tracking Issue #14617</span></span>](https://github.com/dotnet/efcore/issues/14617)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-578">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-578">Old behavior</span></span>

<span data-ttu-id="2771e-579">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="2771e-579">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2771e-580">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="2771e-580">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-581">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-581">New behavior</span></span>

<span data-ttu-id="2771e-582">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="2771e-582">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-583">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-583">Why</span></span>

<span data-ttu-id="2771e-584">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="2771e-584">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-585">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-585">Mitigations</span></span>

<span data-ttu-id="2771e-586">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="2771e-586">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2771e-587">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="2771e-587">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2771e-588">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="2771e-588">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2771e-589">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="2771e-589">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2771e-590">Отслеживание вопроса № 14698</span><span class="sxs-lookup"><span data-stu-id="2771e-590">Tracking Issue #14698</span></span>](https://github.com/dotnet/efcore/issues/14698)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-591">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-591">Old behavior</span></span>

<span data-ttu-id="2771e-592">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="2771e-592">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-593">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-593">New behavior</span></span>

<span data-ttu-id="2771e-594">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="2771e-594">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-595">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-595">Why</span></span>

<span data-ttu-id="2771e-596">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="2771e-596">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-597">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-597">Mitigations</span></span>

<span data-ttu-id="2771e-598">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-598">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2771e-599">Это нетипично.</span><span class="sxs-lookup"><span data-stu-id="2771e-599">This isn't common.</span></span>
<span data-ttu-id="2771e-600">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="2771e-600">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2771e-601">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/dotnet/efcore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="2771e-601">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/dotnet/efcore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2771e-602">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="2771e-602">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2771e-603">Отслеживание вопроса № 12780</span><span class="sxs-lookup"><span data-stu-id="2771e-603">Tracking Issue #12780</span></span>](https://github.com/dotnet/efcore/issues/12780)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-604">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-604">Old behavior</span></span>

<span data-ttu-id="2771e-605">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="2771e-605">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2771e-606">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="2771e-606">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2771e-607">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="2771e-607">In these cases, attempting to lazy-load would be a no-op.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-608">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-608">New behavior</span></span>

<span data-ttu-id="2771e-609">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="2771e-609">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2771e-610">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="2771e-610">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2771e-611">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="2771e-611">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2771e-612">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="2771e-612">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-613">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-613">Why</span></span>

<span data-ttu-id="2771e-614">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2771e-614">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-615">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-615">Mitigations</span></span>

<span data-ttu-id="2771e-616">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="2771e-616">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2771e-617">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2771e-617">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2771e-618">Отслеживание вопроса № 10236</span><span class="sxs-lookup"><span data-stu-id="2771e-618">Tracking Issue #10236</span></span>](https://github.com/dotnet/efcore/issues/10236)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-619">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-619">Old behavior</span></span>

<span data-ttu-id="2771e-620">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="2771e-620">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-621">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-621">New behavior</span></span>

<span data-ttu-id="2771e-622">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="2771e-622">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-623">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-623">Why</span></span>

<span data-ttu-id="2771e-624">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="2771e-624">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-625">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-625">Mitigations</span></span>

<span data-ttu-id="2771e-626">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="2771e-626">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2771e-627">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2771e-627">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2771e-628">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-628">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2771e-629">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="2771e-629">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2771e-630">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="2771e-630">Tracking Issue #9171</span></span>](https://github.com/dotnet/efcore/issues/9171)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-631">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-631">Old behavior</span></span>

<span data-ttu-id="2771e-632">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="2771e-632">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2771e-633">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-633">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2771e-634">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="2771e-634">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2771e-635">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="2771e-635">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-636">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-636">New behavior</span></span>

<span data-ttu-id="2771e-637">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="2771e-637">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-638">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-638">Why</span></span>

<span data-ttu-id="2771e-639">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="2771e-639">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-640">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-640">Mitigations</span></span>

<span data-ttu-id="2771e-641">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="2771e-641">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2771e-642">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="2771e-642">This is not common.</span></span>
<span data-ttu-id="2771e-643">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="2771e-643">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2771e-644">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-644">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2771e-645">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="2771e-645">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2771e-646">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="2771e-646">Tracking Issue #15184</span></span>](https://github.com/dotnet/efcore/issues/15184)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-647">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-647">Old behavior</span></span>

<span data-ttu-id="2771e-648">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="2771e-648">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2771e-649">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="2771e-649">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-650">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-650">New behavior</span></span>

<span data-ttu-id="2771e-651">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="2771e-651">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-652">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-652">Why</span></span>

<span data-ttu-id="2771e-653">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="2771e-653">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-654">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-654">Mitigations</span></span>

<span data-ttu-id="2771e-655">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="2771e-655">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2771e-656">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="2771e-656">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2771e-657">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="2771e-657">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2771e-658">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2771e-658">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2771e-659">Отслеживание вопроса № 9913</span><span class="sxs-lookup"><span data-stu-id="2771e-659">Tracking Issue #9913</span></span>](https://github.com/dotnet/efcore/issues/9913)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-660">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-660">Old behavior</span></span>

<span data-ttu-id="2771e-661">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2771e-661">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-662">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-662">New behavior</span></span>

<span data-ttu-id="2771e-663">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2771e-663">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-664">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-664">Why</span></span>

<span data-ttu-id="2771e-665">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="2771e-665">Type mappings are now used for more than just relational database providers.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-666">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-666">Mitigations</span></span>

<span data-ttu-id="2771e-667">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="2771e-667">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2771e-668">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="2771e-668">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2771e-669">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="2771e-669">ToTable on a derived type throws an exception</span></span>

[<span data-ttu-id="2771e-670">Отслеживание вопроса № 11811</span><span class="sxs-lookup"><span data-stu-id="2771e-670">Tracking Issue #11811</span></span>](https://github.com/dotnet/efcore/issues/11811)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-671">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-671">Old behavior</span></span>

<span data-ttu-id="2771e-672">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="2771e-672">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-673">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-673">New behavior</span></span>

<span data-ttu-id="2771e-674">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="2771e-674">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-675">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-675">Why</span></span>

<span data-ttu-id="2771e-676">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="2771e-676">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2771e-677">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="2771e-677">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-678">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-678">Mitigations</span></span>

<span data-ttu-id="2771e-679">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="2771e-679">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2771e-680">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="2771e-680">ForSqlServerHasIndex replaced with HasIndex</span></span>

[<span data-ttu-id="2771e-681">Отслеживание вопроса № 12366</span><span class="sxs-lookup"><span data-stu-id="2771e-681">Tracking Issue #12366</span></span>](https://github.com/dotnet/efcore/issues/12366)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-682">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-682">Old behavior</span></span>

<span data-ttu-id="2771e-683">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="2771e-683">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-684">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-684">New behavior</span></span>

<span data-ttu-id="2771e-685">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="2771e-685">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2771e-686">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="2771e-686">Use `HasIndex().ForSqlServerInclude()`.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-687">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-687">Why</span></span>

<span data-ttu-id="2771e-688">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="2771e-688">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-689">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-689">Mitigations</span></span>

<span data-ttu-id="2771e-690">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="2771e-690">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2771e-691">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="2771e-691">Metadata API changes</span></span>

[<span data-ttu-id="2771e-692">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="2771e-692">Tracking Issue #214</span></span>](https://github.com/dotnet/efcore/issues/214)

#### <a name="new-behavior"></a><span data-ttu-id="2771e-693">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-693">New behavior</span></span>

<span data-ttu-id="2771e-694">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="2771e-694">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

#### <a name="why"></a><span data-ttu-id="2771e-695">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-695">Why</span></span>

<span data-ttu-id="2771e-696">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="2771e-696">This change simplifies the implementation of the aforementioned interfaces.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-697">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-697">Mitigations</span></span>

<span data-ttu-id="2771e-698">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="2771e-698">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2771e-699">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="2771e-699">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2771e-700">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="2771e-700">Tracking Issue #214</span></span>](https://github.com/dotnet/efcore/issues/214)

#### <a name="new-behavior"></a><span data-ttu-id="2771e-701">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-701">New behavior</span></span>

<span data-ttu-id="2771e-702">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="2771e-702">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

#### <a name="why"></a><span data-ttu-id="2771e-703">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-703">Why</span></span>

<span data-ttu-id="2771e-704">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="2771e-704">This change simplifies the implementation of the aforementioned extension methods.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-705">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-705">Mitigations</span></span>

<span data-ttu-id="2771e-706">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="2771e-706">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2771e-707">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="2771e-707">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2771e-708">Отслеживание вопроса № 12151</span><span class="sxs-lookup"><span data-stu-id="2771e-708">Tracking Issue #12151</span></span>](https://github.com/dotnet/efcore/issues/12151)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-709">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-709">Old behavior</span></span>

<span data-ttu-id="2771e-710">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="2771e-710">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-711">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-711">New behavior</span></span>

<span data-ttu-id="2771e-712">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="2771e-712">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-713">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-713">Why</span></span>

<span data-ttu-id="2771e-714">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="2771e-714">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-715">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-715">Mitigations</span></span>

<span data-ttu-id="2771e-716">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-716">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2771e-717">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="2771e-717">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="2771e-718">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2771e-718">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="2771e-719">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-719">Old behavior</span></span>

<span data-ttu-id="2771e-720">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="2771e-720">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-721">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-721">New behavior</span></span>

<span data-ttu-id="2771e-722">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="2771e-722">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-723">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-723">Why</span></span>

<span data-ttu-id="2771e-724">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="2771e-724">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-725">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-725">Mitigations</span></span>

<span data-ttu-id="2771e-726">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="2771e-726">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2771e-727">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="2771e-727">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2771e-728">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="2771e-728">Tracking Issue #15078</span></span>](https://github.com/dotnet/efcore/issues/15078)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-729">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-729">Old behavior</span></span>

<span data-ttu-id="2771e-730">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="2771e-730">Guid values were previously stored as BLOB values on SQLite.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-731">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-731">New behavior</span></span>

<span data-ttu-id="2771e-732">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="2771e-732">Guid values are now stored as TEXT.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-733">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-733">Why</span></span>

<span data-ttu-id="2771e-734">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="2771e-734">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2771e-735">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="2771e-735">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-736">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-736">Mitigations</span></span>

<span data-ttu-id="2771e-737">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2771e-737">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="2771e-738">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="2771e-738">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

```csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2771e-739">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="2771e-739">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2771e-740">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="2771e-740">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2771e-741">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="2771e-741">Tracking Issue #15020</span></span>](https://github.com/dotnet/efcore/issues/15020)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-742">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-742">Old behavior</span></span>

<span data-ttu-id="2771e-743">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="2771e-743">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2771e-744">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="2771e-744">For example, a char value of *A* was stored as the integer value 65.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-745">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-745">New behavior</span></span>

<span data-ttu-id="2771e-746">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="2771e-746">Char values are now stored as TEXT.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-747">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-747">Why</span></span>

<span data-ttu-id="2771e-748">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="2771e-748">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-749">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-749">Mitigations</span></span>

<span data-ttu-id="2771e-750">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2771e-750">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

```sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2771e-751">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="2771e-751">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

```csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2771e-752">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="2771e-752">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2771e-753">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="2771e-753">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2771e-754">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="2771e-754">Tracking Issue #12978</span></span>](https://github.com/dotnet/efcore/issues/12978)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-755">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-755">Old behavior</span></span>

<span data-ttu-id="2771e-756">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="2771e-756">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-757">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-757">New behavior</span></span>

<span data-ttu-id="2771e-758">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="2771e-758">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-759">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-759">Why</span></span>

<span data-ttu-id="2771e-760">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="2771e-760">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2771e-761">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="2771e-761">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-762">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-762">Mitigations</span></span>

<span data-ttu-id="2771e-763">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="2771e-763">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2771e-764">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="2771e-764">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2771e-765">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="2771e-765">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

```diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2771e-766">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="2771e-766">The Migrations history table also needs to be updated.</span></span>

```sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2771e-767">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="2771e-767">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2771e-768">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="2771e-768">Tracking Issue #16400</span></span>](https://github.com/dotnet/efcore/issues/16400)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-769">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-769">Old behavior</span></span>

<span data-ttu-id="2771e-770">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="2771e-770">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-771">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-771">New behavior</span></span>

<span data-ttu-id="2771e-772">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2771e-772">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-773">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-773">Why</span></span>

<span data-ttu-id="2771e-774">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="2771e-774">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-775">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-775">Mitigations</span></span>

<span data-ttu-id="2771e-776">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="2771e-776">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2771e-777">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/dotnet/efcore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="2771e-777">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/dotnet/efcore/issues/16400) with details.</span></span> <span data-ttu-id="2771e-778">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="2771e-778">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2771e-779">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="2771e-779">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2771e-780">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="2771e-780">Tracking Issue #16119</span></span>](https://github.com/dotnet/efcore/issues/16119)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-781">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-781">Old behavior</span></span>

<span data-ttu-id="2771e-782">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="2771e-782">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-783">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-783">New behavior</span></span>

<span data-ttu-id="2771e-784">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="2771e-784">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-785">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-785">Why</span></span>

<span data-ttu-id="2771e-786">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="2771e-786">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2771e-787">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="2771e-787">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-788">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-788">Mitigations</span></span>

<span data-ttu-id="2771e-789">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="2771e-789">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2771e-790">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-790">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2771e-791">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="2771e-791">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2771e-792">Отслеживание вопроса № 10985</span><span class="sxs-lookup"><span data-stu-id="2771e-792">Tracking Issue #10985</span></span>](https://github.com/dotnet/efcore/issues/10985)

#### <a name="change"></a><span data-ttu-id="2771e-793">Change</span><span class="sxs-lookup"><span data-stu-id="2771e-793">Change</span></span>

<span data-ttu-id="2771e-794">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="2771e-794">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-795">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-795">Why</span></span>

<span data-ttu-id="2771e-796">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="2771e-796">Aligns the naming of this warning event with all other warning events.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-797">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-797">Mitigations</span></span>

<span data-ttu-id="2771e-798">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="2771e-798">Use the new name.</span></span> <span data-ttu-id="2771e-799">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="2771e-799">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2771e-800">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="2771e-800">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2771e-801">Отслеживание вопроса № 10730</span><span class="sxs-lookup"><span data-stu-id="2771e-801">Tracking Issue #10730</span></span>](https://github.com/dotnet/efcore/issues/10730)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-802">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-802">Old behavior</span></span>

<span data-ttu-id="2771e-803">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="2771e-803">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2771e-804">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-804">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

#### <a name="new-behavior"></a><span data-ttu-id="2771e-805">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-805">New behavior</span></span>

<span data-ttu-id="2771e-806">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="2771e-806">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2771e-807">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-807">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

#### <a name="why"></a><span data-ttu-id="2771e-808">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-808">Why</span></span>

<span data-ttu-id="2771e-809">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="2771e-809">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-810">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-810">Mitigations</span></span>

<span data-ttu-id="2771e-811">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="2771e-811">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2771e-812">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="2771e-812">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2771e-813">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="2771e-813">Tracking Issue #15997</span></span>](https://github.com/dotnet/efcore/issues/15997)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-814">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-814">Old behavior</span></span>

<span data-ttu-id="2771e-815">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="2771e-815">Before EF Core 3.0, these methods were protected.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-816">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-816">New behavior</span></span>

<span data-ttu-id="2771e-817">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="2771e-817">Starting with EF Core 3.0, these methods are public.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-818">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-818">Why</span></span>

<span data-ttu-id="2771e-819">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="2771e-819">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2771e-820">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="2771e-820">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-821">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-821">Mitigations</span></span>

<span data-ttu-id="2771e-822">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="2771e-822">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2771e-823">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2771e-823">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2771e-824">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="2771e-824">Tracking Issue #11506</span></span>](https://github.com/dotnet/efcore/issues/11506)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-825">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-825">Old behavior</span></span>

<span data-ttu-id="2771e-826">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="2771e-826">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-827">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-827">New behavior</span></span>

<span data-ttu-id="2771e-828">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2771e-828">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2771e-829">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="2771e-829">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-830">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-830">Why</span></span>

<span data-ttu-id="2771e-831">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="2771e-831">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2771e-832">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="2771e-832">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2771e-833">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2771e-833">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-834">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-834">Mitigations</span></span>

<span data-ttu-id="2771e-835">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="2771e-835">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="2771e-836">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="2771e-836">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="2771e-837">Такую явную ссылку необходимо добавить в любой проект, где требуются типы из пакета.</span><span class="sxs-lookup"><span data-stu-id="2771e-837">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2771e-838">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2771e-838">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2771e-839">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="2771e-839">Tracking Issue #14824</span></span>](https://github.com/dotnet/efcore/issues/14824)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-840">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-840">Old behavior</span></span>

<span data-ttu-id="2771e-841">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="2771e-841">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-842">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-842">New behavior</span></span>

<span data-ttu-id="2771e-843">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2771e-843">We've updated our package to depend on version 2.0.0.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-844">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-844">Why</span></span>

<span data-ttu-id="2771e-845">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2771e-845">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2771e-846">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="2771e-846">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-847">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-847">Mitigations</span></span>

<span data-ttu-id="2771e-848">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="2771e-848">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2771e-849">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="2771e-849">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2771e-850">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2771e-850">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2771e-851">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="2771e-851">Tracking Issue #14825</span></span>](https://github.com/dotnet/efcore/issues/14825)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-852">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-852">Old behavior</span></span>

<span data-ttu-id="2771e-853">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="2771e-853">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-854">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-854">New behavior</span></span>

<span data-ttu-id="2771e-855">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2771e-855">We've update our package to depend on version 2.0.0.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-856">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-856">Why</span></span>

<span data-ttu-id="2771e-857">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="2771e-857">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-858">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-858">Mitigations</span></span>

<span data-ttu-id="2771e-859">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="2771e-859">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2771e-860">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="2771e-860">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="2771e-861">Вместо System.Data.SqlClient используется Microsoft.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="2771e-861">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="2771e-862">Отслеживание вопроса № 15636</span><span class="sxs-lookup"><span data-stu-id="2771e-862">Tracking Issue #15636</span></span>](https://github.com/dotnet/efcore/issues/15636)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-863">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-863">Old behavior</span></span>

<span data-ttu-id="2771e-864">Пакет Microsoft.EntityFrameworkCore.SqlServer ранее зависел от System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2771e-864">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="2771e-865">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-865">New behavior</span></span>

<span data-ttu-id="2771e-866">Мы обновили пакет, и теперь он зависит от Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2771e-866">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-867">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-867">Why</span></span>

<span data-ttu-id="2771e-868">Microsoft.Data.SqlClient теперь является основным разрабатываемым драйвером для доступа к SQL Server вместо System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2771e-868">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="2771e-869">Некоторые важные функции, такие как Always Encrypted, доступны только в Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2771e-869">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-870">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-870">Mitigations</span></span>

<span data-ttu-id="2771e-871">Если код включает прямую зависимость от System.Data.SqlClient, необходимо изменить код так, чтобы он ссылался на Microsoft.Data.SqlClient. Поскольку API двух пакетов очень хорошо совместимы, достаточно изменить пакет и пространство имен.</span><span class="sxs-lookup"><span data-stu-id="2771e-871">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2771e-872">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="2771e-872">Multiple ambiguous self-referencing relationships must be configured</span></span>

[<span data-ttu-id="2771e-873">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="2771e-873">Tracking Issue #13573</span></span>](https://github.com/dotnet/efcore/issues/13573)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-874">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-874">Old behavior</span></span>

<span data-ttu-id="2771e-875">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="2771e-875">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2771e-876">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-876">For example:</span></span>

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

#### <a name="new-behavior"></a><span data-ttu-id="2771e-877">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-877">New behavior</span></span>

<span data-ttu-id="2771e-878">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="2771e-878">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-879">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-879">Why</span></span>

<span data-ttu-id="2771e-880">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="2771e-880">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-881">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-881">Mitigations</span></span>

<span data-ttu-id="2771e-882">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="2771e-882">Use full configuration of the relationship.</span></span> <span data-ttu-id="2771e-883">Пример:</span><span class="sxs-lookup"><span data-stu-id="2771e-883">For example:</span></span>

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

### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="2771e-884">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="2771e-884">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="2771e-885">Отслеживание вопроса № 12757</span><span class="sxs-lookup"><span data-stu-id="2771e-885">Tracking Issue #12757</span></span>](https://github.com/dotnet/efcore/issues/12757)

#### <a name="old-behavior"></a><span data-ttu-id="2771e-886">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-886">Old behavior</span></span>

<span data-ttu-id="2771e-887">DbFunction, настроенная со схемой в виде пустой строки, обрабатывалась как встроенная функция без схемы.</span><span class="sxs-lookup"><span data-stu-id="2771e-887">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="2771e-888">Например, следующий код сопоставит функцию CLR `DatePart` со встроенной функцией `DATEPART` в SqlServer.</span><span class="sxs-lookup"><span data-stu-id="2771e-888">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

#### <a name="new-behavior"></a><span data-ttu-id="2771e-889">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="2771e-889">New behavior</span></span>

<span data-ttu-id="2771e-890">Все сопоставления DbFunction считаются сопоставленными с определяемыми пользователем функциями.</span><span class="sxs-lookup"><span data-stu-id="2771e-890">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="2771e-891">Поэтому пустое строковое значение поместит функцию в схему по умолчанию для модели.</span><span class="sxs-lookup"><span data-stu-id="2771e-891">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="2771e-892">Это может быть схема, настроенная явным образом с помощью `modelBuilder.HasDefaultSchema()` текучего API либо `dbo`.</span><span class="sxs-lookup"><span data-stu-id="2771e-892">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

#### <a name="why"></a><span data-ttu-id="2771e-893">Почему</span><span class="sxs-lookup"><span data-stu-id="2771e-893">Why</span></span>

<span data-ttu-id="2771e-894">Ранее эта функция считалась встроенной из-за пустой схемы, но эта логика применима только к SqlServer, где встроенные функции не принадлежат ни одной схеме.</span><span class="sxs-lookup"><span data-stu-id="2771e-894">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

#### <a name="mitigations"></a><span data-ttu-id="2771e-895">Устранение проблем</span><span class="sxs-lookup"><span data-stu-id="2771e-895">Mitigations</span></span>

<span data-ttu-id="2771e-896">Вручную настройте преобразование функции DbFunction, чтобы сопоставить ее со встроенной функцией.</span><span class="sxs-lookup"><span data-stu-id="2771e-896">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```

<a name="netstandard21"></a>

### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20-reverted"></a><span data-ttu-id="2771e-897">~~Версия EF Core 3.0 больше предназначена для .NET Standard 2.1, а не для .NET Standard 2.0~~ отменено</span><span class="sxs-lookup"><span data-stu-id="2771e-897">~~EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0~~ Reverted</span></span>

[<span data-ttu-id="2771e-898">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="2771e-898">Tracking Issue #15498</span></span>](https://github.com/dotnet/efcore/issues/15498)

<span data-ttu-id="2771e-899">Версия EF Core 3.0 была больше предназначена для .NET Standard 2.1. Это критическое изменение, которое исключало поддержку приложений .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2771e-899">EF Core 3.0 targets .NET Standard 2.1, which is a breaking change which excludes .NET Framework applications.</span></span> <span data-ttu-id="2771e-900">Это изменение отменено в EF Core 3.1, и EF Core снова ориентируется на .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2771e-900">EF Core 3.1 reverted this and targets .NET Standard 2.0 again.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2771e-901">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="2771e-901">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2771e-902">Отслеживание вопроса № 14523</span><span class="sxs-lookup"><span data-stu-id="2771e-902">Tracking Issue #14523</span></span>](https://github.com/dotnet/efcore/issues/14523)

<span data-ttu-id="2771e-903">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="2771e-903">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2771e-904">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="2771e-904">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```
