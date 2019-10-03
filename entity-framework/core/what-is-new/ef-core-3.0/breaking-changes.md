---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0dd4c5c4aa1a5d241fb48abf1372a678d0f7a7a3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813617"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="7c630-102">Критические изменения в EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="7c630-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="7c630-103">Указанные ниже изменения API и поведения могут нарушать работу существующих приложений при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="7c630-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="7c630-104">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="7c630-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="7c630-105">Сводка</span><span class="sxs-lookup"><span data-stu-id="7c630-105">Summary</span></span>

| <span data-ttu-id="7c630-106">**Критические изменения**</span><span class="sxs-lookup"><span data-stu-id="7c630-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="7c630-107">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="7c630-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="7c630-108">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="7c630-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="7c630-109">High</span><span class="sxs-lookup"><span data-stu-id="7c630-109">High</span></span>       |
| [<span data-ttu-id="7c630-110">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="7c630-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="7c630-111">High</span><span class="sxs-lookup"><span data-stu-id="7c630-111">High</span></span>      |
| [<span data-ttu-id="7c630-112">Программа командной строки EF Core "dotnet ef" больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="7c630-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="7c630-113">High</span><span class="sxs-lookup"><span data-stu-id="7c630-113">High</span></span>      |
| [<span data-ttu-id="7c630-114">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="7c630-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="7c630-115">High</span><span class="sxs-lookup"><span data-stu-id="7c630-115">High</span></span>      |
| [<span data-ttu-id="7c630-116">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="7c630-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="7c630-117">High</span><span class="sxs-lookup"><span data-stu-id="7c630-117">High</span></span>      |
| [<span data-ttu-id="7c630-118">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="7c630-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="7c630-119">High</span><span class="sxs-lookup"><span data-stu-id="7c630-119">High</span></span>      |
| [<span data-ttu-id="7c630-120">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c630-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="7c630-121">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-121">Medium</span></span>      |
| [<span data-ttu-id="7c630-122">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="7c630-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="7c630-123">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-123">Medium</span></span>      |
| [<span data-ttu-id="7c630-124">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="7c630-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="7c630-125">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-125">Medium</span></span>      |
| [<span data-ttu-id="7c630-126">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="7c630-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="7c630-127">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-127">Medium</span></span>      |
| [<span data-ttu-id="7c630-128">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="7c630-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="7c630-129">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-129">Medium</span></span>      |
| [<span data-ttu-id="7c630-130">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="7c630-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="7c630-131">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-131">Medium</span></span>      |
| [<span data-ttu-id="7c630-132">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="7c630-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="7c630-133">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-133">Medium</span></span>      |
| [<span data-ttu-id="7c630-134">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="7c630-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="7c630-135">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-135">Medium</span></span>      |
| [<span data-ttu-id="7c630-136">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="7c630-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="7c630-137">Средняя</span><span class="sxs-lookup"><span data-stu-id="7c630-137">Medium</span></span>      |
| [<span data-ttu-id="7c630-138">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="7c630-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="7c630-139">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-139">Low</span></span>      |
| [<span data-ttu-id="7c630-140">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="7c630-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="7c630-141">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-141">Low</span></span>      |
| [<span data-ttu-id="7c630-142">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="7c630-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="7c630-143">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-143">Low</span></span>      |
| [<span data-ttu-id="7c630-144">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="7c630-144">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="7c630-145">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-145">Low</span></span>      |
| [<span data-ttu-id="7c630-146">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="7c630-146">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="7c630-147">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-147">Low</span></span>      |
| [<span data-ttu-id="7c630-148">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="7c630-148">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="7c630-149">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-149">Low</span></span>      |
| [<span data-ttu-id="7c630-150">Соглашение для свойства внешнего ключа больше не сопоставляет то же имя, что у свойства субъекта</span><span class="sxs-lookup"><span data-stu-id="7c630-150">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="7c630-151">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-151">Low</span></span>      |
| [<span data-ttu-id="7c630-152">Подключение к базе данных теперь закрывается, если оно больше не используется, до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="7c630-152">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="7c630-153">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-153">Low</span></span>      |
| [<span data-ttu-id="7c630-154">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="7c630-154">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="7c630-155">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-155">Low</span></span>      |
| [<span data-ttu-id="7c630-156">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="7c630-156">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="7c630-157">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-157">Low</span></span>      |
| [<span data-ttu-id="7c630-158">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="7c630-158">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="7c630-159">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-159">Low</span></span>      |
| [<span data-ttu-id="7c630-160">AddDbContext/AddDbContextPool больше не вызывает методы AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="7c630-160">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="7c630-161">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-161">Low</span></span>      |
| [<span data-ttu-id="7c630-162">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="7c630-162">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="7c630-163">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-163">Low</span></span>      |
| [<span data-ttu-id="7c630-164">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7c630-164">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="7c630-165">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-165">Low</span></span>      |
| [<span data-ttu-id="7c630-166">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="7c630-166">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="7c630-167">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-167">Low</span></span>      |
| [<span data-ttu-id="7c630-168">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="7c630-168">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="7c630-169">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-169">Low</span></span>      |
| [<span data-ttu-id="7c630-170">Создание слишком многих внутренних поставщиков служб теперь по умолчанию является ошибкой</span><span class="sxs-lookup"><span data-stu-id="7c630-170">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="7c630-171">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-171">Low</span></span>      |
| [<span data-ttu-id="7c630-172">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="7c630-172">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="7c630-173">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-173">Low</span></span>      |
| [<span data-ttu-id="7c630-174">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="7c630-174">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="7c630-175">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-175">Low</span></span>      |
| [<span data-ttu-id="7c630-176">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="7c630-176">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="7c630-177">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-177">Low</span></span>      |
| [<span data-ttu-id="7c630-178">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="7c630-178">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="7c630-179">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-179">Low</span></span>      |
| [<span data-ttu-id="7c630-180">EF Core больше не отправляет PRAGMA для принудительного применения внешних ключей SQLite</span><span class="sxs-lookup"><span data-stu-id="7c630-180">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="7c630-181">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-181">Low</span></span>      |
| [<span data-ttu-id="7c630-182">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="7c630-182">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="7c630-183">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-183">Low</span></span>      |
| [<span data-ttu-id="7c630-184">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="7c630-184">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="7c630-185">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-185">Low</span></span>      |
| [<span data-ttu-id="7c630-186">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="7c630-186">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="7c630-187">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-187">Low</span></span>      |
| [<span data-ttu-id="7c630-188">Идентификаторы миграции теперь создаются с использованием календаря инвариантных языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="7c630-188">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="7c630-189">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-189">Low</span></span>      |
| [<span data-ttu-id="7c630-190">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="7c630-190">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="7c630-191">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-191">Low</span></span>      |
| [<span data-ttu-id="7c630-192">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="7c630-192">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="7c630-193">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-193">Low</span></span>      |
| [<span data-ttu-id="7c630-194">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="7c630-194">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="7c630-195">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-195">Low</span></span>      |
| [<span data-ttu-id="7c630-196">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="7c630-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="7c630-197">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-197">Low</span></span>      |
| [<span data-ttu-id="7c630-198">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="7c630-198">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="7c630-199">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-199">Low</span></span>      |
| [<span data-ttu-id="7c630-200">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7c630-200">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="7c630-201">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-201">Low</span></span>      |
| [<span data-ttu-id="7c630-202">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7c630-202">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="7c630-203">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-203">Low</span></span>      |
| [<span data-ttu-id="7c630-204">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="7c630-204">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="7c630-205">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-205">Low</span></span>      |
| [<span data-ttu-id="7c630-206">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="7c630-206">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="7c630-207">Low</span><span class="sxs-lookup"><span data-stu-id="7c630-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="7c630-208">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="7c630-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="7c630-209">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="7c630-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="7c630-210">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-210">**Old behavior**</span></span>

<span data-ttu-id="7c630-211">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="7c630-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="7c630-212">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="7c630-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="7c630-213">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-213">**New behavior**</span></span>

<span data-ttu-id="7c630-214">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="7c630-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="7c630-215">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="7c630-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="7c630-216">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-216">**Why**</span></span>

<span data-ttu-id="7c630-217">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="7c630-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="7c630-218">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="7c630-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="7c630-219">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="7c630-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="7c630-220">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="7c630-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="7c630-221">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="7c630-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="7c630-222">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="7c630-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="7c630-223">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-223">**Mitigations**</span></span>

<span data-ttu-id="7c630-224">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="7c630-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="7c630-225">EF Core 3.0 больше предназначен для .NET Standard 2.1, а не для .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="7c630-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="7c630-226">Отслеживание вопроса № 15498</span><span class="sxs-lookup"><span data-stu-id="7c630-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="7c630-227">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-227">**Old behavior**</span></span>

<span data-ttu-id="7c630-228">До версии 3.0 EF Core был предназначен для .NET Standard 2.0 и выполнялся на всех платформах, поддерживающих этот стандарт, включая .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7c630-228">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="7c630-229">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-229">**New behavior**</span></span>

<span data-ttu-id="7c630-230">Начиная с версии 3.0, EF Core предназначен для .NET Standard 2.1 и выполняется на всех платформах, поддерживающих этот стандарт.</span><span class="sxs-lookup"><span data-stu-id="7c630-230">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="7c630-231">Сюда не входит .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7c630-231">This does not include .NET Framework.</span></span>

<span data-ttu-id="7c630-232">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-232">**Why**</span></span>

<span data-ttu-id="7c630-233">Это часть стратегического решения по технологиям .NET, направленного на работу над .NET Core и другими современными платформами .NET, такими как Xamarin.</span><span class="sxs-lookup"><span data-stu-id="7c630-233">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="7c630-234">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-234">**Mitigations**</span></span>

<span data-ttu-id="7c630-235">Рассмотрите возможность перехода на современные платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="7c630-235">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="7c630-236">Если это невозможно, продолжайте использовать EF Core 2.1 или EF Core 2.2, которые поддерживают .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7c630-236">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="7c630-237">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c630-237">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="7c630-238">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="7c630-238">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="7c630-239">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-239">**Old behavior**</span></span>

<span data-ttu-id="7c630-240">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c630-240">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="7c630-241">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-241">**New behavior**</span></span>

<span data-ttu-id="7c630-242">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-242">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="7c630-243">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-243">**Why**</span></span>

<span data-ttu-id="7c630-244">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c630-244">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="7c630-245">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="7c630-245">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="7c630-246">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="7c630-246">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="7c630-247">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-247">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="7c630-248">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-248">**Mitigations**</span></span>

<span data-ttu-id="7c630-249">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="7c630-249">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="7c630-250">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="7c630-250">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="7c630-251">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="7c630-251">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="7c630-252">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-252">**Old behavior**</span></span>

<span data-ttu-id="7c630-253">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="7c630-253">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="7c630-254">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-254">**New behavior**</span></span>

<span data-ttu-id="7c630-255">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="7c630-255">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="7c630-256">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-256">**Why**</span></span>

<span data-ttu-id="7c630-257">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="7c630-257">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="7c630-258">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-258">**Mitigations**</span></span>

<span data-ttu-id="7c630-259">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="7c630-259">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="7c630-260">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="7c630-260">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="7c630-261">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="7c630-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="7c630-262">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="7c630-262">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="7c630-263">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-263">**Old behavior**</span></span>

<span data-ttu-id="7c630-264">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="7c630-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="7c630-265">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-265">**New behavior**</span></span>

<span data-ttu-id="7c630-266">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7c630-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="7c630-267">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-267">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="7c630-268">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="7c630-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="7c630-269">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-269">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="7c630-270">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="7c630-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="7c630-271">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-271">**Why**</span></span>

<span data-ttu-id="7c630-272">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="7c630-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="7c630-273">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="7c630-273">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="7c630-274">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-274">**Mitigations**</span></span>

<span data-ttu-id="7c630-275">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="7c630-275">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="7c630-276">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="7c630-276">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="7c630-277">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="7c630-277">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="7c630-278">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-278">**Old behavior**</span></span>

<span data-ttu-id="7c630-279">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="7c630-279">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="7c630-280">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-280">**New behavior**</span></span>

<span data-ttu-id="7c630-281">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="7c630-281">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="7c630-282">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="7c630-282">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="7c630-283">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-283">**Why**</span></span>

<span data-ttu-id="7c630-284">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="7c630-284">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="7c630-285">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-285">**Mitigations**</span></span>

<span data-ttu-id="7c630-286">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="7c630-286">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="7c630-287">Запросы без отслеживания больше не выполняют разрешение идентификаторов</span><span class="sxs-lookup"><span data-stu-id="7c630-287">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="7c630-288">Отслеживание вопроса № 13518</span><span class="sxs-lookup"><span data-stu-id="7c630-288">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="7c630-289">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-289">**Old behavior**</span></span>

<span data-ttu-id="7c630-290">До EF Core 3.0 один и тот же экземпляр сущности будет использоваться для каждого вхождения сущности с заданным типом и ИД.</span><span class="sxs-lookup"><span data-stu-id="7c630-290">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="7c630-291">Это соответствует поведению запросов отслеживания.</span><span class="sxs-lookup"><span data-stu-id="7c630-291">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="7c630-292">Например, запрос</span><span class="sxs-lookup"><span data-stu-id="7c630-292">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="7c630-293">возвращает тот же экземпляр `Category` для каждого `Product`, связанного с заданной категорией.</span><span class="sxs-lookup"><span data-stu-id="7c630-293">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="7c630-294">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-294">**New behavior**</span></span>

<span data-ttu-id="7c630-295">Начиная с EF Core 3.0, при обнаружении сущности с заданным типом и ИД в разных местах возвращенного графа будут создаваться различные экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="7c630-295">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="7c630-296">Например, вышеприведенный запрос теперь будет возвращать новый экземпляр `Category` для каждого `Product`, даже если два продукта связаны с одной категорией.</span><span class="sxs-lookup"><span data-stu-id="7c630-296">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="7c630-297">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-297">**Why**</span></span>

<span data-ttu-id="7c630-298">Разрешение идентификаторов (то есть определение того, что сущность имеет тот же тип и идентификатор, что и обнаруженная ранее сущность) добавляет дополнительную нагрузку на производительность и память.</span><span class="sxs-lookup"><span data-stu-id="7c630-298">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="7c630-299">Обычно это идет вразрез с причинами использования запросов без отслеживания.</span><span class="sxs-lookup"><span data-stu-id="7c630-299">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="7c630-300">Кроме того, хотя разрешение идентификаторов иногда может быть полезным, оно не требуется, если сущности должны быть сериализованы и отправлены клиенту, что является распространенным случаем для неотслеживаемых запросов.</span><span class="sxs-lookup"><span data-stu-id="7c630-300">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="7c630-301">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-301">**Mitigations**</span></span>

<span data-ttu-id="7c630-302">Используйте запрос с отслеживанием, если требуется разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="7c630-302">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="7c630-303">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="7c630-303">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="7c630-304">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="7c630-304">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="7c630-305">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="7c630-305">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="7c630-306">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="7c630-306">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="7c630-307">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="7c630-307">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="7c630-308">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="7c630-308">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="7c630-309">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-309">**Old behavior**</span></span>

<span data-ttu-id="7c630-310">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="7c630-310">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="7c630-311">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="7c630-311">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="7c630-312">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-312">**New behavior**</span></span>

<span data-ttu-id="7c630-313">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="7c630-313">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="7c630-314">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-314">**Why**</span></span>

<span data-ttu-id="7c630-315">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7c630-315">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="7c630-316">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-316">**Mitigations**</span></span>

<span data-ttu-id="7c630-317">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="7c630-317">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="7c630-318">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7c630-318">This can be avoided by:</span></span>
* <span data-ttu-id="7c630-319">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="7c630-319">Not using store-generated keys.</span></span>
* <span data-ttu-id="7c630-320">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7c630-320">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="7c630-321">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="7c630-321">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="7c630-322">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="7c630-322">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="7c630-323">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="7c630-323">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="7c630-324">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="7c630-324">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="7c630-325">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-325">**Old behavior**</span></span>

<span data-ttu-id="7c630-326">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7c630-326">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="7c630-327">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-327">**New behavior**</span></span>

<span data-ttu-id="7c630-328">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="7c630-328">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="7c630-329">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7c630-329">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="7c630-330">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="7c630-330">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="7c630-331">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-331">**Why**</span></span>

<span data-ttu-id="7c630-332">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="7c630-332">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="7c630-333">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-333">**Mitigations**</span></span>

<span data-ttu-id="7c630-334">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="7c630-334">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="7c630-335">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="7c630-335">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="7c630-336">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="7c630-336">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="7c630-337">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="7c630-337">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="7c630-338">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="7c630-338">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="7c630-339">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="7c630-339">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="7c630-340">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-340">**Old behavior**</span></span>

<span data-ttu-id="7c630-341">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7c630-341">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="7c630-342">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-342">**New behavior**</span></span>

<span data-ttu-id="7c630-343">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="7c630-343">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="7c630-344">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="7c630-344">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="7c630-345">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-345">**Why**</span></span>

<span data-ttu-id="7c630-346">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7c630-346">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="7c630-347">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-347">**Mitigations**</span></span>

<span data-ttu-id="7c630-348">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="7c630-348">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="7c630-349">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-349">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="7c630-350">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="7c630-350">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="7c630-351">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="7c630-351">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="7c630-352">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-352">**Old behavior**</span></span>

<span data-ttu-id="7c630-353">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="7c630-353">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="7c630-354">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-354">**New behavior**</span></span>

<span data-ttu-id="7c630-355">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="7c630-355">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="7c630-356">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-356">**Why**</span></span>

<span data-ttu-id="7c630-357">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="7c630-357">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="7c630-358">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-358">**Mitigations**</span></span>

<span data-ttu-id="7c630-359">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="7c630-359">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="7c630-360">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="7c630-360">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="7c630-361">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="7c630-361">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="7c630-362">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-362">**Old behavior**</span></span>

<span data-ttu-id="7c630-363">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/keyless-entity-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="7c630-363">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="7c630-364">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="7c630-364">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="7c630-365">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-365">**New behavior**</span></span>

<span data-ttu-id="7c630-366">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="7c630-366">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="7c630-367">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="7c630-367">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="7c630-368">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-368">**Why**</span></span>

<span data-ttu-id="7c630-369">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="7c630-369">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="7c630-370">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="7c630-370">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="7c630-371">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="7c630-371">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="7c630-372">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-372">**Mitigations**</span></span>

<span data-ttu-id="7c630-373">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="7c630-373">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="7c630-374">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="7c630-374">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="7c630-375">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="7c630-375">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="7c630-376">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="7c630-376">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="7c630-377">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="7c630-377">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="7c630-378">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="7c630-378">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="7c630-379">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="7c630-379">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="7c630-380">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-380">**Old behavior**</span></span>

<span data-ttu-id="7c630-381">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="7c630-381">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="7c630-382">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-382">**New behavior**</span></span>

<span data-ttu-id="7c630-383">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="7c630-383">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="7c630-384">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-384">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="7c630-385">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="7c630-385">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="7c630-386">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="7c630-386">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="7c630-387">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-387">For example:</span></span>

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

<span data-ttu-id="7c630-388">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="7c630-388">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="7c630-389">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-389">**Why**</span></span>

<span data-ttu-id="7c630-390">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="7c630-390">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="7c630-391">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="7c630-391">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="7c630-392">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-392">**Mitigations**</span></span>

<span data-ttu-id="7c630-393">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="7c630-393">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="7c630-394">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="7c630-394">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="7c630-395">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="7c630-395">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="7c630-396">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-396">**Old behavior**</span></span>

<span data-ttu-id="7c630-397">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="7c630-397">Consider the following model:</span></span>
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
<span data-ttu-id="7c630-398">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="7c630-398">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="7c630-399">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-399">**New behavior**</span></span>

<span data-ttu-id="7c630-400">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="7c630-400">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="7c630-401">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="7c630-401">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="7c630-402">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-402">**Mitigations**</span></span>

<span data-ttu-id="7c630-403">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="7c630-403">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="7c630-404">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="7c630-404">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="7c630-405">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="7c630-405">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="7c630-406">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="7c630-406">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="7c630-407">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-407">**Old behavior**</span></span>

<span data-ttu-id="7c630-408">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="7c630-408">Consider the following model:</span></span>
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
<span data-ttu-id="7c630-409">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="7c630-409">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="7c630-410">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-410">**New behavior**</span></span>

<span data-ttu-id="7c630-411">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="7c630-411">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="7c630-412">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="7c630-412">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="7c630-413">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-413">**Why**</span></span>

<span data-ttu-id="7c630-414">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="7c630-414">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="7c630-415">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-415">**Mitigations**</span></span>

<span data-ttu-id="7c630-416">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="7c630-416">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="7c630-417">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="7c630-417">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="7c630-418">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="7c630-418">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="7c630-419">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="7c630-419">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="7c630-420">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-420">**Old behavior**</span></span>

<span data-ttu-id="7c630-421">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="7c630-421">Consider the following model:</span></span>
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

<span data-ttu-id="7c630-422">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7c630-422">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="7c630-423">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-423">**New behavior**</span></span>

<span data-ttu-id="7c630-424">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="7c630-424">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="7c630-425">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-425">**Why**</span></span>

<span data-ttu-id="7c630-426">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="7c630-426">The old behavoir was unexpected.</span></span>

<span data-ttu-id="7c630-427">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-427">**Mitigations**</span></span>

<span data-ttu-id="7c630-428">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="7c630-428">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="7c630-429">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="7c630-429">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="7c630-430">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="7c630-430">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="7c630-431">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-431">**Old behavior**</span></span>

<span data-ttu-id="7c630-432">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="7c630-432">Consider the following model:</span></span>
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
<span data-ttu-id="7c630-433">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="7c630-433">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="7c630-434">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="7c630-434">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="7c630-435">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-435">**New behavior**</span></span>

<span data-ttu-id="7c630-436">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="7c630-436">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="7c630-437">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="7c630-437">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="7c630-438">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-438">For example:</span></span>

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

<span data-ttu-id="7c630-439">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-439">**Why**</span></span>

<span data-ttu-id="7c630-440">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="7c630-440">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="7c630-441">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-441">**Mitigations**</span></span>

<span data-ttu-id="7c630-442">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="7c630-442">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="7c630-443">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="7c630-443">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="7c630-444">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="7c630-444">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="7c630-445">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-445">**Old behavior**</span></span>

<span data-ttu-id="7c630-446">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="7c630-446">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="7c630-447">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-447">**New behavior**</span></span>

<span data-ttu-id="7c630-448">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="7c630-448">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="7c630-449">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-449">**Why**</span></span>

<span data-ttu-id="7c630-450">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="7c630-450">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="7c630-451">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="7c630-451">The new behavior also matches EF6.</span></span>

<span data-ttu-id="7c630-452">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-452">**Mitigations**</span></span>

<span data-ttu-id="7c630-453">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="7c630-453">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="7c630-454">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="7c630-454">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="7c630-455">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="7c630-455">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="7c630-456">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-456">**Old behavior**</span></span>

<span data-ttu-id="7c630-457">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="7c630-457">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="7c630-458">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-458">**New behavior**</span></span>

<span data-ttu-id="7c630-459">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="7c630-459">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="7c630-460">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="7c630-460">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="7c630-461">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-461">**Why**</span></span>

<span data-ttu-id="7c630-462">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="7c630-462">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="7c630-463">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-463">**Mitigations**</span></span>

<span data-ttu-id="7c630-464">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="7c630-464">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="7c630-465">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="7c630-465">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="7c630-466">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="7c630-466">Backing fields are used by default</span></span>

[<span data-ttu-id="7c630-467">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="7c630-467">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="7c630-468">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-468">**Old behavior**</span></span>

<span data-ttu-id="7c630-469">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="7c630-469">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="7c630-470">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="7c630-470">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="7c630-471">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-471">**New behavior**</span></span>

<span data-ttu-id="7c630-472">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="7c630-472">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="7c630-473">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="7c630-473">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="7c630-474">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-474">**Why**</span></span>

<span data-ttu-id="7c630-475">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="7c630-475">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="7c630-476">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-476">**Mitigations**</span></span>

<span data-ttu-id="7c630-477">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7c630-477">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="7c630-478">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-478">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="7c630-479">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="7c630-479">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="7c630-480">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="7c630-480">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="7c630-481">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-481">**Old behavior**</span></span>

<span data-ttu-id="7c630-482">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="7c630-482">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="7c630-483">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="7c630-483">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="7c630-484">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-484">**New behavior**</span></span>

<span data-ttu-id="7c630-485">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="7c630-485">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="7c630-486">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-486">**Why**</span></span>

<span data-ttu-id="7c630-487">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="7c630-487">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="7c630-488">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-488">**Mitigations**</span></span>

<span data-ttu-id="7c630-489">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="7c630-489">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="7c630-490">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="7c630-490">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="7c630-491">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="7c630-491">Field-only property names should match the field name</span></span>

<span data-ttu-id="7c630-492">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-492">**Old behavior**</span></span>

<span data-ttu-id="7c630-493">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе .NET, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="7c630-493">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="7c630-494">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-494">**New behavior**</span></span>

<span data-ttu-id="7c630-495">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="7c630-495">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="7c630-496">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-496">**Why**</span></span>

<span data-ttu-id="7c630-497">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="7c630-497">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="7c630-498">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-498">**Mitigations**</span></span>

<span data-ttu-id="7c630-499">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="7c630-499">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="7c630-500">В будущем выпуске EF Core после версии 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства (см. вопрос [№ 15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="7c630-500">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="7c630-501">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="7c630-501">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="7c630-502">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="7c630-502">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="7c630-503">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-503">**Old behavior**</span></span>

<span data-ttu-id="7c630-504">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="7c630-504">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="7c630-505">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-505">**New behavior**</span></span>

<span data-ttu-id="7c630-506">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7c630-506">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="7c630-507">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-507">**Why**</span></span>

<span data-ttu-id="7c630-508">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="7c630-508">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="7c630-509">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-509">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="7c630-510">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-510">**Mitigations**</span></span>

<span data-ttu-id="7c630-511">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="7c630-511">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="7c630-512">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="7c630-512">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="7c630-513">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="7c630-513">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="7c630-514">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-514">**Old behavior**</span></span>

<span data-ttu-id="7c630-515">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="7c630-515">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="7c630-516">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="7c630-516">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="7c630-517">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-517">**New behavior**</span></span>

<span data-ttu-id="7c630-518">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="7c630-518">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="7c630-519">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="7c630-519">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="7c630-520">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="7c630-520">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="7c630-521">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="7c630-521">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="7c630-522">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-522">**Why**</span></span>

<span data-ttu-id="7c630-523">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="7c630-523">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="7c630-524">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-524">**Mitigations**</span></span>

<span data-ttu-id="7c630-525">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="7c630-525">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="7c630-526">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7c630-526">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="7c630-527">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="7c630-527">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="7c630-528">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-528">**Old behavior**</span></span>

<span data-ttu-id="7c630-529">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="7c630-529">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="7c630-530">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="7c630-530">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="7c630-531">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-531">**New behavior**</span></span>

<span data-ttu-id="7c630-532">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="7c630-532">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="7c630-533">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-533">**Why**</span></span>

<span data-ttu-id="7c630-534">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="7c630-534">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="7c630-535">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-535">**Mitigations**</span></span>

<span data-ttu-id="7c630-536">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="7c630-536">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="7c630-537">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="7c630-537">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="7c630-538">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="7c630-538">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="7c630-539">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="7c630-539">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="7c630-540">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="7c630-540">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="7c630-541">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-541">**Old behavior**</span></span>

<span data-ttu-id="7c630-542">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="7c630-542">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="7c630-543">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-543">**New behavior**</span></span>

<span data-ttu-id="7c630-544">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="7c630-544">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="7c630-545">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-545">**Why**</span></span>

<span data-ttu-id="7c630-546">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="7c630-546">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="7c630-547">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-547">**Mitigations**</span></span>

<span data-ttu-id="7c630-548">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-548">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="7c630-549">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="7c630-549">This isn't common.</span></span>
<span data-ttu-id="7c630-550">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="7c630-550">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="7c630-551">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="7c630-551">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="7c630-552">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="7c630-552">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="7c630-553">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="7c630-553">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="7c630-554">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-554">**Old behavior**</span></span>

<span data-ttu-id="7c630-555">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="7c630-555">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="7c630-556">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="7c630-556">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="7c630-557">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="7c630-557">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="7c630-558">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-558">**New behavior**</span></span>

<span data-ttu-id="7c630-559">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="7c630-559">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="7c630-560">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="7c630-560">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="7c630-561">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="7c630-561">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="7c630-562">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="7c630-562">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="7c630-563">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-563">**Why**</span></span>

<span data-ttu-id="7c630-564">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7c630-564">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="7c630-565">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-565">**Mitigations**</span></span>

<span data-ttu-id="7c630-566">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="7c630-566">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="7c630-567">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7c630-567">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="7c630-568">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="7c630-568">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="7c630-569">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-569">**Old behavior**</span></span>

<span data-ttu-id="7c630-570">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="7c630-570">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="7c630-571">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-571">**New behavior**</span></span>

<span data-ttu-id="7c630-572">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="7c630-572">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="7c630-573">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-573">**Why**</span></span>

<span data-ttu-id="7c630-574">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="7c630-574">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="7c630-575">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-575">**Mitigations**</span></span>

<span data-ttu-id="7c630-576">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="7c630-576">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="7c630-577">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7c630-577">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="7c630-578">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-578">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="7c630-579">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="7c630-579">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="7c630-580">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="7c630-580">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="7c630-581">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-581">**Old behavior**</span></span>

<span data-ttu-id="7c630-582">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="7c630-582">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="7c630-583">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-583">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="7c630-584">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="7c630-584">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="7c630-585">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="7c630-585">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="7c630-586">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-586">**New behavior**</span></span>

<span data-ttu-id="7c630-587">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="7c630-587">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="7c630-588">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-588">**Why**</span></span>

<span data-ttu-id="7c630-589">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="7c630-589">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="7c630-590">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-590">**Mitigations**</span></span>

<span data-ttu-id="7c630-591">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="7c630-591">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="7c630-592">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="7c630-592">This is not common.</span></span>
<span data-ttu-id="7c630-593">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="7c630-593">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="7c630-594">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-594">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="7c630-595">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="7c630-595">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="7c630-596">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="7c630-596">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="7c630-597">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-597">**Old behavior**</span></span>

<span data-ttu-id="7c630-598">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="7c630-598">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="7c630-599">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="7c630-599">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="7c630-600">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-600">**New behavior**</span></span>

<span data-ttu-id="7c630-601">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="7c630-601">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="7c630-602">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-602">**Why**</span></span>

<span data-ttu-id="7c630-603">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="7c630-603">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="7c630-604">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-604">**Mitigations**</span></span>

<span data-ttu-id="7c630-605">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="7c630-605">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="7c630-606">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="7c630-606">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="7c630-607">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="7c630-607">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="7c630-608">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="7c630-608">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="7c630-609">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="7c630-609">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="7c630-610">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-610">**Old behavior**</span></span>

<span data-ttu-id="7c630-611">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="7c630-611">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="7c630-612">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-612">**New behavior**</span></span>

<span data-ttu-id="7c630-613">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="7c630-613">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="7c630-614">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-614">**Why**</span></span>

<span data-ttu-id="7c630-615">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="7c630-615">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="7c630-616">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-616">**Mitigations**</span></span>

<span data-ttu-id="7c630-617">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="7c630-617">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="7c630-618">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="7c630-618">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="7c630-619">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="7c630-619">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="7c630-620">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="7c630-620">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="7c630-621">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-621">**Old behavior**</span></span>

<span data-ttu-id="7c630-622">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="7c630-622">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="7c630-623">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-623">**New behavior**</span></span>

<span data-ttu-id="7c630-624">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="7c630-624">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="7c630-625">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-625">**Why**</span></span>

<span data-ttu-id="7c630-626">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="7c630-626">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="7c630-627">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="7c630-627">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="7c630-628">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-628">**Mitigations**</span></span>

<span data-ttu-id="7c630-629">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="7c630-629">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="7c630-630">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="7c630-630">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="7c630-631">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="7c630-631">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="7c630-632">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-632">**Old behavior**</span></span>

<span data-ttu-id="7c630-633">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="7c630-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="7c630-634">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-634">**New behavior**</span></span>

<span data-ttu-id="7c630-635">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="7c630-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="7c630-636">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="7c630-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="7c630-637">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-637">**Why**</span></span>

<span data-ttu-id="7c630-638">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="7c630-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="7c630-639">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-639">**Mitigations**</span></span>

<span data-ttu-id="7c630-640">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="7c630-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="7c630-641">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="7c630-641">Metadata API changes</span></span>

[<span data-ttu-id="7c630-642">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="7c630-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="7c630-643">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-643">**New behavior**</span></span>

<span data-ttu-id="7c630-644">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="7c630-644">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="7c630-645">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-645">**Why**</span></span>

<span data-ttu-id="7c630-646">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="7c630-646">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="7c630-647">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-647">**Mitigations**</span></span>

<span data-ttu-id="7c630-648">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="7c630-648">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="7c630-649">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="7c630-649">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="7c630-650">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="7c630-650">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="7c630-651">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-651">**New behavior**</span></span>

<span data-ttu-id="7c630-652">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="7c630-652">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="7c630-653">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-653">**Why**</span></span>

<span data-ttu-id="7c630-654">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="7c630-654">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="7c630-655">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-655">**Mitigations**</span></span>

<span data-ttu-id="7c630-656">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="7c630-656">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="7c630-657">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="7c630-657">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="7c630-658">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="7c630-658">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="7c630-659">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-659">**Old behavior**</span></span>

<span data-ttu-id="7c630-660">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c630-660">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="7c630-661">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-661">**New behavior**</span></span>

<span data-ttu-id="7c630-662">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c630-662">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="7c630-663">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-663">**Why**</span></span>

<span data-ttu-id="7c630-664">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="7c630-664">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="7c630-665">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-665">**Mitigations**</span></span>

<span data-ttu-id="7c630-666">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-666">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="7c630-667">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="7c630-667">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="7c630-668">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="7c630-668">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="7c630-669">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-669">**Old behavior**</span></span>

<span data-ttu-id="7c630-670">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="7c630-670">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="7c630-671">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-671">**New behavior**</span></span>

<span data-ttu-id="7c630-672">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="7c630-672">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="7c630-673">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-673">**Why**</span></span>

<span data-ttu-id="7c630-674">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="7c630-674">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="7c630-675">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-675">**Mitigations**</span></span>

<span data-ttu-id="7c630-676">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="7c630-676">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="7c630-677">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="7c630-677">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="7c630-678">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="7c630-678">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="7c630-679">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-679">**Old behavior**</span></span>

<span data-ttu-id="7c630-680">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="7c630-680">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="7c630-681">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-681">**New behavior**</span></span>

<span data-ttu-id="7c630-682">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="7c630-682">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="7c630-683">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-683">**Why**</span></span>

<span data-ttu-id="7c630-684">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="7c630-684">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="7c630-685">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="7c630-685">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="7c630-686">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-686">**Mitigations**</span></span>

<span data-ttu-id="7c630-687">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7c630-687">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="7c630-688">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="7c630-688">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="7c630-689">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="7c630-689">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="7c630-690">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="7c630-690">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="7c630-691">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="7c630-691">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="7c630-692">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-692">**Old behavior**</span></span>

<span data-ttu-id="7c630-693">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="7c630-693">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="7c630-694">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="7c630-694">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="7c630-695">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-695">**New behavior**</span></span>

<span data-ttu-id="7c630-696">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="7c630-696">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="7c630-697">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-697">**Why**</span></span>

<span data-ttu-id="7c630-698">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="7c630-698">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="7c630-699">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-699">**Mitigations**</span></span>

<span data-ttu-id="7c630-700">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="7c630-700">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="7c630-701">В EF Core можно и дальше использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="7c630-701">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="7c630-702">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="7c630-702">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="7c630-703">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="7c630-703">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="7c630-704">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="7c630-704">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="7c630-705">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-705">**Old behavior**</span></span>

<span data-ttu-id="7c630-706">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="7c630-706">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="7c630-707">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-707">**New behavior**</span></span>

<span data-ttu-id="7c630-708">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="7c630-708">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="7c630-709">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-709">**Why**</span></span>

<span data-ttu-id="7c630-710">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="7c630-710">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="7c630-711">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="7c630-711">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="7c630-712">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-712">**Mitigations**</span></span>

<span data-ttu-id="7c630-713">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="7c630-713">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="7c630-714">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="7c630-714">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="7c630-715">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="7c630-715">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="7c630-716">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="7c630-716">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="7c630-717">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="7c630-717">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="7c630-718">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="7c630-718">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="7c630-719">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-719">**Old behavior**</span></span>

<span data-ttu-id="7c630-720">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="7c630-720">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="7c630-721">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-721">**New behavior**</span></span>

<span data-ttu-id="7c630-722">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c630-722">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="7c630-723">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-723">**Why**</span></span>

<span data-ttu-id="7c630-724">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="7c630-724">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="7c630-725">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-725">**Mitigations**</span></span>

<span data-ttu-id="7c630-726">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="7c630-726">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="7c630-727">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/aspnet/EntityFrameworkCore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="7c630-727">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="7c630-728">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="7c630-728">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="7c630-729">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="7c630-729">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="7c630-730">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="7c630-730">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="7c630-731">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-731">**Old behavior**</span></span>

<span data-ttu-id="7c630-732">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="7c630-732">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="7c630-733">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-733">**New behavior**</span></span>

<span data-ttu-id="7c630-734">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="7c630-734">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="7c630-735">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-735">**Why**</span></span>

<span data-ttu-id="7c630-736">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="7c630-736">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="7c630-737">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="7c630-737">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="7c630-738">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-738">**Mitigations**</span></span>

<span data-ttu-id="7c630-739">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="7c630-739">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="7c630-740">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-740">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="7c630-741">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="7c630-741">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="7c630-742">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="7c630-742">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="7c630-743">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="7c630-743">**Change**</span></span>

<span data-ttu-id="7c630-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="7c630-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="7c630-745">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-745">**Why**</span></span>

<span data-ttu-id="7c630-746">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="7c630-746">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="7c630-747">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-747">**Mitigations**</span></span>

<span data-ttu-id="7c630-748">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="7c630-748">Use the new name.</span></span> <span data-ttu-id="7c630-749">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="7c630-749">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="7c630-750">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="7c630-750">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="7c630-751">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="7c630-751">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="7c630-752">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-752">**Old behavior**</span></span>

<span data-ttu-id="7c630-753">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="7c630-753">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="7c630-754">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-754">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="7c630-755">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-755">**New behavior**</span></span>

<span data-ttu-id="7c630-756">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="7c630-756">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="7c630-757">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-757">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="7c630-758">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-758">**Why**</span></span>

<span data-ttu-id="7c630-759">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="7c630-759">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="7c630-760">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-760">**Mitigations**</span></span>

<span data-ttu-id="7c630-761">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="7c630-761">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="7c630-762">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="7c630-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="7c630-763">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="7c630-763">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="7c630-764">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-764">**Old behavior**</span></span>

<span data-ttu-id="7c630-765">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="7c630-765">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="7c630-766">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-766">**New behavior**</span></span>

<span data-ttu-id="7c630-767">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="7c630-767">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="7c630-768">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-768">**Why**</span></span>

<span data-ttu-id="7c630-769">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="7c630-769">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="7c630-770">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="7c630-770">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="7c630-771">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-771">**Mitigations**</span></span>

<span data-ttu-id="7c630-772">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="7c630-772">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="7c630-773">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="7c630-773">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="7c630-774">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="7c630-774">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="7c630-775">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-775">**Old behavior**</span></span>

<span data-ttu-id="7c630-776">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="7c630-776">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="7c630-777">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-777">**New behavior**</span></span>

<span data-ttu-id="7c630-778">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="7c630-778">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="7c630-779">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="7c630-779">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="7c630-780">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-780">**Why**</span></span>

<span data-ttu-id="7c630-781">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="7c630-781">This package is only intended to be used at design time.</span></span> <span data-ttu-id="7c630-782">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="7c630-782">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="7c630-783">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="7c630-783">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="7c630-784">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-784">**Mitigations**</span></span>

<span data-ttu-id="7c630-785">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="7c630-785">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="7c630-786">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="7c630-786">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="7c630-787">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7c630-787">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="7c630-788">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="7c630-788">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="7c630-789">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-789">**Old behavior**</span></span>

<span data-ttu-id="7c630-790">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="7c630-790">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="7c630-791">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-791">**New behavior**</span></span>

<span data-ttu-id="7c630-792">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="7c630-792">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="7c630-793">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-793">**Why**</span></span>

<span data-ttu-id="7c630-794">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="7c630-794">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="7c630-795">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="7c630-795">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="7c630-796">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-796">**Mitigations**</span></span>

<span data-ttu-id="7c630-797">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="7c630-797">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="7c630-798">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="7c630-798">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="7c630-799">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7c630-799">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="7c630-800">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="7c630-800">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="7c630-801">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-801">**Old behavior**</span></span>

<span data-ttu-id="7c630-802">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="7c630-802">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="7c630-803">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-803">**New behavior**</span></span>

<span data-ttu-id="7c630-804">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="7c630-804">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="7c630-805">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-805">**Why**</span></span>

<span data-ttu-id="7c630-806">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c630-806">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="7c630-807">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-807">**Mitigations**</span></span>

<span data-ttu-id="7c630-808">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="7c630-808">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="7c630-809">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="7c630-809">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="7c630-810">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="7c630-810">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="7c630-811">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="7c630-811">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="7c630-812">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-812">**Old behavior**</span></span>

<span data-ttu-id="7c630-813">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="7c630-813">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="7c630-814">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-814">For example:</span></span>

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

<span data-ttu-id="7c630-815">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-815">**New behavior**</span></span>

<span data-ttu-id="7c630-816">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="7c630-816">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="7c630-817">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-817">**Why**</span></span>

<span data-ttu-id="7c630-818">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="7c630-818">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="7c630-819">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-819">**Mitigations**</span></span>

<span data-ttu-id="7c630-820">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="7c630-820">Use full configuration of the relationship.</span></span> <span data-ttu-id="7c630-821">Например:</span><span class="sxs-lookup"><span data-stu-id="7c630-821">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="7c630-822">DbFunction.Schema в виде нулевой или пустой строки выполняет настройку для включения в схему по умолчанию для модели</span><span class="sxs-lookup"><span data-stu-id="7c630-822">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="7c630-823">Отслеживание вопроса № 12757</span><span class="sxs-lookup"><span data-stu-id="7c630-823">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="7c630-824">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-824">**Old behavior**</span></span>

<span data-ttu-id="7c630-825">DbFunction, настроенная со схемой в виде пустой строки, обрабатывалась как встроенная функция без схемы.</span><span class="sxs-lookup"><span data-stu-id="7c630-825">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="7c630-826">Например, следующий код сопоставит функцию CLR `DatePart` со встроенной функцией `DATEPART` в SqlServer.</span><span class="sxs-lookup"><span data-stu-id="7c630-826">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="7c630-827">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="7c630-827">**New behavior**</span></span>

<span data-ttu-id="7c630-828">Все сопоставления DbFunction считаются сопоставленными с определяемыми пользователем функциями.</span><span class="sxs-lookup"><span data-stu-id="7c630-828">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="7c630-829">Поэтому пустое строковое значение поместит функцию в схему по умолчанию для модели.</span><span class="sxs-lookup"><span data-stu-id="7c630-829">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="7c630-830">Это может быть схема, настроенная явным образом с помощью `modelBuilder.HasDefaultSchema()` текучего API либо `dbo`.</span><span class="sxs-lookup"><span data-stu-id="7c630-830">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="7c630-831">**Причина**</span><span class="sxs-lookup"><span data-stu-id="7c630-831">**Why**</span></span>

<span data-ttu-id="7c630-832">Ранее эта функция считалась встроенной из-за пустой схемы, но эта логика применима только к SqlServer, где встроенные функции не принадлежат ни одной схеме.</span><span class="sxs-lookup"><span data-stu-id="7c630-832">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="7c630-833">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="7c630-833">**Mitigations**</span></span>

<span data-ttu-id="7c630-834">Вручную настройте преобразование функции DbFunction, чтобы сопоставить ее со встроенной функцией.</span><span class="sxs-lookup"><span data-stu-id="7c630-834">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
