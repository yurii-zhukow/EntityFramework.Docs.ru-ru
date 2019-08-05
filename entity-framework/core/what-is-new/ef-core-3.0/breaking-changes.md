---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497527"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="fc64c-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="fc64c-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc64c-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="fc64c-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="fc64c-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="fc64c-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="fc64c-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="fc64c-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="fc64c-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="fc64c-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="fc64c-107">Сводка</span><span class="sxs-lookup"><span data-stu-id="fc64c-107">Summary</span></span>

| <span data-ttu-id="fc64c-108">**Критические изменения**</span><span class="sxs-lookup"><span data-stu-id="fc64c-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="fc64c-109">**Серьезность**</span><span class="sxs-lookup"><span data-stu-id="fc64c-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="fc64c-110">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="fc64c-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="fc64c-111">Высокая</span><span class="sxs-lookup"><span data-stu-id="fc64c-111">High</span></span>       |
| [<span data-ttu-id="fc64c-112">Программа командной строки EF Core "dotnet ef" больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="fc64c-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="fc64c-113">Высокая</span><span class="sxs-lookup"><span data-stu-id="fc64c-113">High</span></span>      |
| [<span data-ttu-id="fc64c-114">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="fc64c-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="fc64c-115">Высокая</span><span class="sxs-lookup"><span data-stu-id="fc64c-115">High</span></span>      |
| [<span data-ttu-id="fc64c-116">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="fc64c-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="fc64c-117">Высокая</span><span class="sxs-lookup"><span data-stu-id="fc64c-117">High</span></span>      |
| [<span data-ttu-id="fc64c-118">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc64c-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="fc64c-119">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-119">Medium</span></span>      |
| [<span data-ttu-id="fc64c-120">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="fc64c-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="fc64c-121">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-121">Medium</span></span>      |
| [<span data-ttu-id="fc64c-122">Более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="fc64c-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="fc64c-123">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-123">Medium</span></span>      |
| [<span data-ttu-id="fc64c-124">Изменение API конфигурации для отношений типа принадлежности</span><span class="sxs-lookup"><span data-stu-id="fc64c-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="fc64c-125">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-125">Medium</span></span>      |
| [<span data-ttu-id="fc64c-126">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="fc64c-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="fc64c-127">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-127">Medium</span></span>      |
| [<span data-ttu-id="fc64c-128">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="fc64c-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="fc64c-129">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-129">Medium</span></span>      |
| [<span data-ttu-id="fc64c-130">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="fc64c-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="fc64c-131">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-131">Medium</span></span>      |
| [<span data-ttu-id="fc64c-132">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="fc64c-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="fc64c-133">Средняя</span><span class="sxs-lookup"><span data-stu-id="fc64c-133">Medium</span></span>      |
| [<span data-ttu-id="fc64c-134">Методы FromSql можно указать только в корневых элементах запроса</span><span class="sxs-lookup"><span data-stu-id="fc64c-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="fc64c-135">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-135">Low</span></span>      |
| [<span data-ttu-id="fc64c-136">~~Выполнение запроса с ведением журнала на уровне отладки~~ отменено</span><span class="sxs-lookup"><span data-stu-id="fc64c-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="fc64c-137">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-137">Low</span></span>      |
| [<span data-ttu-id="fc64c-138">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="fc64c-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="fc64c-139">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-139">Low</span></span>      |
| [<span data-ttu-id="fc64c-140">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="fc64c-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="fc64c-141">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-141">Low</span></span>      |
| [<span data-ttu-id="fc64c-142">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="fc64c-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="fc64c-143">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-143">Low</span></span>      |
| [<span data-ttu-id="fc64c-144">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="fc64c-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="fc64c-145">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-145">Low</span></span>      |
| [<span data-ttu-id="fc64c-146">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="fc64c-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="fc64c-147">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-147">Low</span></span>      |
| [<span data-ttu-id="fc64c-148">Соглашение для свойства внешнего ключа больше не сопоставляет то же имя, что у свойства субъекта</span><span class="sxs-lookup"><span data-stu-id="fc64c-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="fc64c-149">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-149">Low</span></span>      |
| [<span data-ttu-id="fc64c-150">Подключение к базе данных теперь закрывается, если оно больше не используется, до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="fc64c-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="fc64c-151">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-151">Low</span></span>      |
| [<span data-ttu-id="fc64c-152">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="fc64c-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="fc64c-153">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-153">Low</span></span>      |
| [<span data-ttu-id="fc64c-154">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="fc64c-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="fc64c-155">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-155">Low</span></span>      |
| [<span data-ttu-id="fc64c-156">Имена свойств, доступных только для полей, должны совпадать с именем поля</span><span class="sxs-lookup"><span data-stu-id="fc64c-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="fc64c-157">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-157">Low</span></span>      |
| [<span data-ttu-id="fc64c-158">AddDbContext/AddDbContextPool больше не вызывает методы AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="fc64c-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="fc64c-159">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-159">Low</span></span>      |
| [<span data-ttu-id="fc64c-160">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="fc64c-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="fc64c-161">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-161">Low</span></span>      |
| [<span data-ttu-id="fc64c-162">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fc64c-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="fc64c-163">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-163">Low</span></span>      |
| [<span data-ttu-id="fc64c-164">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="fc64c-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="fc64c-165">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-165">Low</span></span>      |
| [<span data-ttu-id="fc64c-166">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="fc64c-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="fc64c-167">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-167">Low</span></span>      |
| [<span data-ttu-id="fc64c-168">Создание слишком многих внутренних поставщиков служб теперь по умолчанию является ошибкой</span><span class="sxs-lookup"><span data-stu-id="fc64c-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="fc64c-169">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-169">Low</span></span>      |
| [<span data-ttu-id="fc64c-170">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="fc64c-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="fc64c-171">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-171">Low</span></span>      |
| [<span data-ttu-id="fc64c-172">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="fc64c-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="fc64c-173">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-173">Low</span></span>      |
| [<span data-ttu-id="fc64c-174">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="fc64c-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="fc64c-175">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-175">Low</span></span>      |
| [<span data-ttu-id="fc64c-176">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="fc64c-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="fc64c-177">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-177">Low</span></span>      |
| [<span data-ttu-id="fc64c-178">EF Core больше не отправляет PRAGMA для принудительного применения внешних ключей SQLite</span><span class="sxs-lookup"><span data-stu-id="fc64c-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="fc64c-179">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-179">Low</span></span>      |
| [<span data-ttu-id="fc64c-180">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="fc64c-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="fc64c-181">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-181">Low</span></span>      |
| [<span data-ttu-id="fc64c-182">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="fc64c-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="fc64c-183">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-183">Low</span></span>      |
| [<span data-ttu-id="fc64c-184">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="fc64c-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="fc64c-185">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-185">Low</span></span>      |
| [<span data-ttu-id="fc64c-186">Идентификаторы миграции теперь создаются с использованием календаря инвариантных языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="fc64c-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="fc64c-187">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-187">Low</span></span>      |
| [<span data-ttu-id="fc64c-188">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="fc64c-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="fc64c-189">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-189">Low</span></span>      |
| [<span data-ttu-id="fc64c-190">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="fc64c-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="fc64c-191">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-191">Low</span></span>      |
| [<span data-ttu-id="fc64c-192">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="fc64c-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="fc64c-193">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-193">Low</span></span>      |
| [<span data-ttu-id="fc64c-194">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="fc64c-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="fc64c-195">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-195">Low</span></span>      |
| [<span data-ttu-id="fc64c-196">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="fc64c-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="fc64c-197">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-197">Low</span></span>      |
| [<span data-ttu-id="fc64c-198">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="fc64c-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="fc64c-199">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-199">Low</span></span>      |
| [<span data-ttu-id="fc64c-200">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="fc64c-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="fc64c-201">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-201">Low</span></span>      |
| [<span data-ttu-id="fc64c-202">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="fc64c-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="fc64c-203">Низкая</span><span class="sxs-lookup"><span data-stu-id="fc64c-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="fc64c-204">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="fc64c-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="fc64c-205">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="fc64c-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="fc64c-206">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-207">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-207">**Old behavior**</span></span>

<span data-ttu-id="fc64c-208">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="fc64c-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="fc64c-209">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="fc64c-210">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-210">**New behavior**</span></span>

<span data-ttu-id="fc64c-211">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="fc64c-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="fc64c-212">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="fc64c-213">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-213">**Why**</span></span>

<span data-ttu-id="fc64c-214">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="fc64c-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="fc64c-215">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="fc64c-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="fc64c-216">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="fc64c-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="fc64c-217">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="fc64c-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="fc64c-218">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="fc64c-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="fc64c-219">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="fc64c-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="fc64c-220">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-220">**Mitigations**</span></span>

<span data-ttu-id="fc64c-221">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="fc64c-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="fc64c-222">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc64c-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="fc64c-223">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="fc64c-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="fc64c-224">Это изменение реализовано в ASP.NET Core 3.0, предварительная версия 1.</span><span class="sxs-lookup"><span data-stu-id="fc64c-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="fc64c-225">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-225">**Old behavior**</span></span>

<span data-ttu-id="fc64c-226">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc64c-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="fc64c-227">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-227">**New behavior**</span></span>

<span data-ttu-id="fc64c-228">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="fc64c-229">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-229">**Why**</span></span>

<span data-ttu-id="fc64c-230">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc64c-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="fc64c-231">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="fc64c-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="fc64c-232">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="fc64c-233">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="fc64c-234">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-234">**Mitigations**</span></span>

<span data-ttu-id="fc64c-235">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="fc64c-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="fc64c-236">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="fc64c-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="fc64c-237">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="fc64c-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="fc64c-238">Это изменение реализовано в EF Core 3.0, предварительная версия 4, и соответствующей версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="fc64c-239">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-239">**Old behavior**</span></span>

<span data-ttu-id="fc64c-240">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="fc64c-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="fc64c-241">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-241">**New behavior**</span></span>

<span data-ttu-id="fc64c-242">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="fc64c-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="fc64c-243">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-243">**Why**</span></span>

<span data-ttu-id="fc64c-244">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="fc64c-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="fc64c-245">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-245">**Mitigations**</span></span>

<span data-ttu-id="fc64c-246">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` как глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="fc64c-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="fc64c-247">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="fc64c-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="fc64c-248">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="fc64c-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="fc64c-249">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="fc64c-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="fc64c-250">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-251">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-251">**Old behavior**</span></span>

<span data-ttu-id="fc64c-252">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="fc64c-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="fc64c-253">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-253">**New behavior**</span></span>

<span data-ttu-id="fc64c-254">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fc64c-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="fc64c-255">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="fc64c-256">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fc64c-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="fc64c-257">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="fc64c-258">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="fc64c-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="fc64c-259">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-259">**Why**</span></span>

<span data-ttu-id="fc64c-260">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="fc64c-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="fc64c-261">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="fc64c-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="fc64c-262">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-262">**Mitigations**</span></span>

<span data-ttu-id="fc64c-263">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="fc64c-264">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="fc64c-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="fc64c-265">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="fc64c-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="fc64c-266">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="fc64c-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fc64c-267">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-267">**Old behavior**</span></span>

<span data-ttu-id="fc64c-268">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="fc64c-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="fc64c-269">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-269">**New behavior**</span></span>

<span data-ttu-id="fc64c-270">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="fc64c-271">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="fc64c-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="fc64c-272">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-272">**Why**</span></span>

<span data-ttu-id="fc64c-273">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="fc64c-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="fc64c-274">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-274">**Mitigations**</span></span>

<span data-ttu-id="fc64c-275">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="fc64c-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="fc64c-276">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="fc64c-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="fc64c-277">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="fc64c-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="fc64c-278">Это изменение отменено в выпуске EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="fc64c-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fc64c-279">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="fc64c-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="fc64c-280">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="fc64c-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="fc64c-281">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="fc64c-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="fc64c-282">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="fc64c-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="fc64c-283">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="fc64c-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="fc64c-284">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-284">**Old behavior**</span></span>

<span data-ttu-id="fc64c-285">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="fc64c-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="fc64c-286">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="fc64c-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="fc64c-287">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-287">**New behavior**</span></span>

<span data-ttu-id="fc64c-288">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="fc64c-289">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-289">**Why**</span></span>

<span data-ttu-id="fc64c-290">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="fc64c-291">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-291">**Mitigations**</span></span>

<span data-ttu-id="fc64c-292">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="fc64c-293">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fc64c-293">This can be avoided by:</span></span>
* <span data-ttu-id="fc64c-294">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="fc64c-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="fc64c-295">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="fc64c-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="fc64c-296">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="fc64c-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="fc64c-297">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="fc64c-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="fc64c-298">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="fc64c-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="fc64c-299">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="fc64c-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="fc64c-300">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-301">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-301">**Old behavior**</span></span>

<span data-ttu-id="fc64c-302">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="fc64c-303">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-303">**New behavior**</span></span>

<span data-ttu-id="fc64c-304">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="fc64c-305">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="fc64c-306">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="fc64c-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="fc64c-307">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-307">**Why**</span></span>

<span data-ttu-id="fc64c-308">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="fc64c-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="fc64c-309">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-309">**Mitigations**</span></span>

<span data-ttu-id="fc64c-310">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="fc64c-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="fc64c-311">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="fc64c-312">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="fc64c-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="fc64c-313">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="fc64c-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="fc64c-314">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="fc64c-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="fc64c-315">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="fc64c-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="fc64c-316">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-317">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-317">**Old behavior**</span></span>

<span data-ttu-id="fc64c-318">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="fc64c-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="fc64c-319">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-319">**New behavior**</span></span>

<span data-ttu-id="fc64c-320">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="fc64c-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="fc64c-321">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="fc64c-322">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-322">**Why**</span></span>

<span data-ttu-id="fc64c-323">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="fc64c-324">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-324">**Mitigations**</span></span>

<span data-ttu-id="fc64c-325">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="fc64c-326">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="fc64c-327">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="fc64c-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="fc64c-328">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="fc64c-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="fc64c-329">Это изменение реализовано в EF Core 3.0, предварительная версия 5.</span><span class="sxs-lookup"><span data-stu-id="fc64c-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="fc64c-330">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-330">**Old behavior**</span></span>

<span data-ttu-id="fc64c-331">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="fc64c-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="fc64c-332">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-332">**New behavior**</span></span>

<span data-ttu-id="fc64c-333">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="fc64c-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="fc64c-334">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-334">**Why**</span></span>

<span data-ttu-id="fc64c-335">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="fc64c-336">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-336">**Mitigations**</span></span>

<span data-ttu-id="fc64c-337">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="fc64c-338">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="fc64c-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="fc64c-339">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="fc64c-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="fc64c-340">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-341">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-341">**Old behavior**</span></span>

<span data-ttu-id="fc64c-342">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="fc64c-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="fc64c-343">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="fc64c-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="fc64c-344">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-344">**New behavior**</span></span>

<span data-ttu-id="fc64c-345">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="fc64c-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="fc64c-346">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="fc64c-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="fc64c-347">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-347">**Why**</span></span>

<span data-ttu-id="fc64c-348">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="fc64c-349">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="fc64c-350">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="fc64c-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="fc64c-351">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-351">**Mitigations**</span></span>

<span data-ttu-id="fc64c-352">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="fc64c-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="fc64c-353">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="fc64c-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="fc64c-354">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="fc64c-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="fc64c-355">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="fc64c-356">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="fc64c-357">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="fc64c-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="fc64c-358">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="fc64c-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="fc64c-359">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-360">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-360">**Old behavior**</span></span>

<span data-ttu-id="fc64c-361">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="fc64c-362">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-362">**New behavior**</span></span>

<span data-ttu-id="fc64c-363">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="fc64c-364">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="fc64c-365">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="fc64c-366">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="fc64c-367">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-367">For example:</span></span>

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

<span data-ttu-id="fc64c-368">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="fc64c-369">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-369">**Why**</span></span>

<span data-ttu-id="fc64c-370">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="fc64c-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="fc64c-371">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="fc64c-372">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-372">**Mitigations**</span></span>

<span data-ttu-id="fc64c-373">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="fc64c-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="fc64c-374">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="fc64c-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="fc64c-375">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="fc64c-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="fc64c-376">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-377">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-377">**Old behavior**</span></span>

<span data-ttu-id="fc64c-378">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="fc64c-378">Consider the following model:</span></span>
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
<span data-ttu-id="fc64c-379">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="fc64c-380">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-380">**New behavior**</span></span>

<span data-ttu-id="fc64c-381">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="fc64c-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="fc64c-382">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="fc64c-383">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-383">**Mitigations**</span></span>

<span data-ttu-id="fc64c-384">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="fc64c-385">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="fc64c-386">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="fc64c-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="fc64c-387">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="fc64c-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="fc64c-388">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-389">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-389">**Old behavior**</span></span>

<span data-ttu-id="fc64c-390">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="fc64c-390">Consider the following model:</span></span>
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
<span data-ttu-id="fc64c-391">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="fc64c-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="fc64c-392">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-392">**New behavior**</span></span>

<span data-ttu-id="fc64c-393">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="fc64c-394">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="fc64c-395">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-395">**Why**</span></span>

<span data-ttu-id="fc64c-396">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="fc64c-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="fc64c-397">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-397">**Mitigations**</span></span>

<span data-ttu-id="fc64c-398">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="fc64c-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="fc64c-399">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="fc64c-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="fc64c-400">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="fc64c-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="fc64c-401">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="fc64c-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="fc64c-402">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-403">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-403">**Old behavior**</span></span>

<span data-ttu-id="fc64c-404">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="fc64c-404">Consider the following model:</span></span>
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

<span data-ttu-id="fc64c-405">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fc64c-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="fc64c-406">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-406">**New behavior**</span></span>

<span data-ttu-id="fc64c-407">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="fc64c-408">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-408">**Why**</span></span>

<span data-ttu-id="fc64c-409">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="fc64c-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="fc64c-410">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-410">**Mitigations**</span></span>

<span data-ttu-id="fc64c-411">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="fc64c-412">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="fc64c-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="fc64c-413">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="fc64c-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="fc64c-414">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-415">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-415">**Old behavior**</span></span>

<span data-ttu-id="fc64c-416">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="fc64c-416">Consider the following model:</span></span>
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
<span data-ttu-id="fc64c-417">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="fc64c-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="fc64c-418">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="fc64c-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="fc64c-419">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-419">**New behavior**</span></span>

<span data-ttu-id="fc64c-420">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="fc64c-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="fc64c-421">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="fc64c-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="fc64c-422">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-422">For example:</span></span>

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

<span data-ttu-id="fc64c-423">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-423">**Why**</span></span>

<span data-ttu-id="fc64c-424">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="fc64c-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="fc64c-425">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-425">**Mitigations**</span></span>

<span data-ttu-id="fc64c-426">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="fc64c-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="fc64c-427">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="fc64c-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="fc64c-428">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="fc64c-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="fc64c-429">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-430">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-430">**Old behavior**</span></span>

<span data-ttu-id="fc64c-431">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="fc64c-432">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-432">**New behavior**</span></span>

<span data-ttu-id="fc64c-433">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="fc64c-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="fc64c-434">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-434">**Why**</span></span>

<span data-ttu-id="fc64c-435">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="fc64c-436">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="fc64c-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="fc64c-437">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-437">**Mitigations**</span></span>

<span data-ttu-id="fc64c-438">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="fc64c-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="fc64c-439">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="fc64c-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="fc64c-440">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="fc64c-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="fc64c-441">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-442">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-442">**Old behavior**</span></span>

<span data-ttu-id="fc64c-443">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="fc64c-444">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-444">**New behavior**</span></span>

<span data-ttu-id="fc64c-445">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="fc64c-446">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="fc64c-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="fc64c-447">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-447">**Why**</span></span>

<span data-ttu-id="fc64c-448">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="fc64c-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="fc64c-449">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-449">**Mitigations**</span></span>

<span data-ttu-id="fc64c-450">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="fc64c-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="fc64c-451">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="fc64c-452">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="fc64c-452">Backing fields are used by default</span></span>

[<span data-ttu-id="fc64c-453">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="fc64c-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="fc64c-454">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="fc64c-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="fc64c-455">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-455">**Old behavior**</span></span>

<span data-ttu-id="fc64c-456">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="fc64c-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="fc64c-457">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="fc64c-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="fc64c-458">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-458">**New behavior**</span></span>

<span data-ttu-id="fc64c-459">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="fc64c-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="fc64c-460">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="fc64c-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="fc64c-461">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-461">**Why**</span></span>

<span data-ttu-id="fc64c-462">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="fc64c-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="fc64c-463">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-463">**Mitigations**</span></span>

<span data-ttu-id="fc64c-464">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="fc64c-465">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="fc64c-466">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="fc64c-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="fc64c-467">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="fc64c-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="fc64c-468">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-469">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-469">**Old behavior**</span></span>

<span data-ttu-id="fc64c-470">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="fc64c-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="fc64c-471">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="fc64c-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="fc64c-472">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-472">**New behavior**</span></span>

<span data-ttu-id="fc64c-473">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="fc64c-474">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-474">**Why**</span></span>

<span data-ttu-id="fc64c-475">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="fc64c-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="fc64c-476">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-476">**Mitigations**</span></span>

<span data-ttu-id="fc64c-477">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="fc64c-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="fc64c-478">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="fc64c-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="fc64c-479">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="fc64c-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="fc64c-480">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-481">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-481">**Old behavior**</span></span>

<span data-ttu-id="fc64c-482">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе CLR, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="fc64c-483">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-483">**New behavior**</span></span>

<span data-ttu-id="fc64c-484">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="fc64c-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="fc64c-485">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-485">**Why**</span></span>

<span data-ttu-id="fc64c-486">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="fc64c-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="fc64c-487">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-487">**Mitigations**</span></span>

<span data-ttu-id="fc64c-488">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="fc64c-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="fc64c-489">В последующей предварительной версии EF Core 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства:</span><span class="sxs-lookup"><span data-stu-id="fc64c-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="fc64c-490">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="fc64c-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="fc64c-491">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="fc64c-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="fc64c-492">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-493">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-493">**Old behavior**</span></span>

<span data-ttu-id="fc64c-494">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="fc64c-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="fc64c-495">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-495">**New behavior**</span></span>

<span data-ttu-id="fc64c-496">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="fc64c-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="fc64c-497">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-497">**Why**</span></span>

<span data-ttu-id="fc64c-498">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="fc64c-499">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="fc64c-500">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-500">**Mitigations**</span></span>

<span data-ttu-id="fc64c-501">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="fc64c-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="fc64c-502">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="fc64c-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="fc64c-503">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="fc64c-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="fc64c-504">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-505">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-505">**Old behavior**</span></span>

<span data-ttu-id="fc64c-506">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="fc64c-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="fc64c-507">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="fc64c-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="fc64c-508">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-508">**New behavior**</span></span>

<span data-ttu-id="fc64c-509">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="fc64c-510">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="fc64c-511">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="fc64c-512">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="fc64c-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="fc64c-513">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-513">**Why**</span></span>

<span data-ttu-id="fc64c-514">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="fc64c-515">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-515">**Mitigations**</span></span>

<span data-ttu-id="fc64c-516">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="fc64c-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="fc64c-517">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fc64c-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="fc64c-518">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="fc64c-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="fc64c-519">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-520">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-520">**Old behavior**</span></span>

<span data-ttu-id="fc64c-521">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="fc64c-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="fc64c-522">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="fc64c-523">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-523">**New behavior**</span></span>

<span data-ttu-id="fc64c-524">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="fc64c-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="fc64c-525">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-525">**Why**</span></span>

<span data-ttu-id="fc64c-526">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="fc64c-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="fc64c-527">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-527">**Mitigations**</span></span>

<span data-ttu-id="fc64c-528">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="fc64c-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="fc64c-529">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="fc64c-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="fc64c-530">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="fc64c-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="fc64c-531">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="fc64c-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="fc64c-532">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="fc64c-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="fc64c-533">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-534">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-534">**Old behavior**</span></span>

<span data-ttu-id="fc64c-535">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="fc64c-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="fc64c-536">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-536">**New behavior**</span></span>

<span data-ttu-id="fc64c-537">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="fc64c-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="fc64c-538">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-538">**Why**</span></span>

<span data-ttu-id="fc64c-539">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="fc64c-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="fc64c-540">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-540">**Mitigations**</span></span>

<span data-ttu-id="fc64c-541">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="fc64c-542">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="fc64c-542">This isn't common.</span></span>
<span data-ttu-id="fc64c-543">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="fc64c-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="fc64c-544">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="fc64c-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="fc64c-545">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="fc64c-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="fc64c-546">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="fc64c-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="fc64c-547">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-548">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-548">**Old behavior**</span></span>

<span data-ttu-id="fc64c-549">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="fc64c-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="fc64c-550">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="fc64c-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="fc64c-551">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="fc64c-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="fc64c-552">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-552">**New behavior**</span></span>

<span data-ttu-id="fc64c-553">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="fc64c-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="fc64c-554">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="fc64c-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="fc64c-555">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="fc64c-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="fc64c-556">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="fc64c-557">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-557">**Why**</span></span>

<span data-ttu-id="fc64c-558">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="fc64c-559">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-559">**Mitigations**</span></span>

<span data-ttu-id="fc64c-560">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="fc64c-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="fc64c-561">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="fc64c-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="fc64c-562">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="fc64c-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="fc64c-563">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-564">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-564">**Old behavior**</span></span>

<span data-ttu-id="fc64c-565">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="fc64c-566">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-566">**New behavior**</span></span>

<span data-ttu-id="fc64c-567">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="fc64c-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="fc64c-568">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-568">**Why**</span></span>

<span data-ttu-id="fc64c-569">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="fc64c-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="fc64c-570">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-570">**Mitigations**</span></span>

<span data-ttu-id="fc64c-571">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="fc64c-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="fc64c-572">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="fc64c-573">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="fc64c-574">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="fc64c-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="fc64c-575">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="fc64c-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="fc64c-576">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-577">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-577">**Old behavior**</span></span>

<span data-ttu-id="fc64c-578">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="fc64c-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="fc64c-579">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="fc64c-580">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="fc64c-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="fc64c-581">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="fc64c-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="fc64c-582">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-582">**New behavior**</span></span>

<span data-ttu-id="fc64c-583">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="fc64c-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="fc64c-584">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-584">**Why**</span></span>

<span data-ttu-id="fc64c-585">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="fc64c-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="fc64c-586">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-586">**Mitigations**</span></span>

<span data-ttu-id="fc64c-587">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="fc64c-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="fc64c-588">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="fc64c-588">This is not common.</span></span>
<span data-ttu-id="fc64c-589">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="fc64c-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="fc64c-590">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="fc64c-591">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="fc64c-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="fc64c-592">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="fc64c-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="fc64c-593">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-594">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-594">**Old behavior**</span></span>

<span data-ttu-id="fc64c-595">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="fc64c-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="fc64c-596">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="fc64c-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="fc64c-597">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-597">**New behavior**</span></span>

<span data-ttu-id="fc64c-598">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="fc64c-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="fc64c-599">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-599">**Why**</span></span>

<span data-ttu-id="fc64c-600">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="fc64c-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="fc64c-601">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-601">**Mitigations**</span></span>

<span data-ttu-id="fc64c-602">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="fc64c-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="fc64c-603">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="fc64c-604">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="fc64c-605">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="fc64c-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="fc64c-606">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="fc64c-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="fc64c-607">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="fc64c-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="fc64c-608">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-608">**Old behavior**</span></span>

<span data-ttu-id="fc64c-609">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="fc64c-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="fc64c-610">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-610">**New behavior**</span></span>

<span data-ttu-id="fc64c-611">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="fc64c-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="fc64c-612">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-612">**Why**</span></span>

<span data-ttu-id="fc64c-613">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="fc64c-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="fc64c-614">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-614">**Mitigations**</span></span>

<span data-ttu-id="fc64c-615">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="fc64c-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="fc64c-616">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="fc64c-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="fc64c-617">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="fc64c-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="fc64c-618">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="fc64c-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="fc64c-619">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-620">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-620">**Old behavior**</span></span>

<span data-ttu-id="fc64c-621">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="fc64c-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="fc64c-622">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-622">**New behavior**</span></span>

<span data-ttu-id="fc64c-623">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="fc64c-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="fc64c-624">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-624">**Why**</span></span>

<span data-ttu-id="fc64c-625">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="fc64c-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="fc64c-626">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="fc64c-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="fc64c-627">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-627">**Mitigations**</span></span>

<span data-ttu-id="fc64c-628">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="fc64c-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="fc64c-629">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="fc64c-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="fc64c-630">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="fc64c-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="fc64c-631">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-632">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-632">**Old behavior**</span></span>

<span data-ttu-id="fc64c-633">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="fc64c-634">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-634">**New behavior**</span></span>

<span data-ttu-id="fc64c-635">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="fc64c-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="fc64c-636">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="fc64c-637">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-637">**Why**</span></span>

<span data-ttu-id="fc64c-638">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="fc64c-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="fc64c-639">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-639">**Mitigations**</span></span>

<span data-ttu-id="fc64c-640">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="fc64c-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="fc64c-641">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="fc64c-641">Metadata API changes</span></span>

[<span data-ttu-id="fc64c-642">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="fc64c-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="fc64c-643">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-644">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-644">**New behavior**</span></span>

<span data-ttu-id="fc64c-645">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="fc64c-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="fc64c-646">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-646">**Why**</span></span>

<span data-ttu-id="fc64c-647">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="fc64c-648">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-648">**Mitigations**</span></span>

<span data-ttu-id="fc64c-649">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="fc64c-650">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="fc64c-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="fc64c-651">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="fc64c-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="fc64c-652">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="fc64c-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fc64c-653">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-653">**New behavior**</span></span>

<span data-ttu-id="fc64c-654">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="fc64c-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="fc64c-655">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-655">**Why**</span></span>

<span data-ttu-id="fc64c-656">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="fc64c-657">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-657">**Mitigations**</span></span>

<span data-ttu-id="fc64c-658">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="fc64c-659">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="fc64c-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="fc64c-660">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="fc64c-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="fc64c-661">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="fc64c-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fc64c-662">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-662">**Old behavior**</span></span>

<span data-ttu-id="fc64c-663">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="fc64c-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="fc64c-664">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-664">**New behavior**</span></span>

<span data-ttu-id="fc64c-665">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="fc64c-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="fc64c-666">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-666">**Why**</span></span>

<span data-ttu-id="fc64c-667">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="fc64c-668">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-668">**Mitigations**</span></span>

<span data-ttu-id="fc64c-669">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="fc64c-670">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="fc64c-671">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="fc64c-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="fc64c-672">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-672">**Old behavior**</span></span>

<span data-ttu-id="fc64c-673">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="fc64c-674">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-674">**New behavior**</span></span>

<span data-ttu-id="fc64c-675">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="fc64c-676">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-676">**Why**</span></span>

<span data-ttu-id="fc64c-677">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="fc64c-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="fc64c-678">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-678">**Mitigations**</span></span>

<span data-ttu-id="fc64c-679">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="fc64c-680">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="fc64c-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="fc64c-681">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="fc64c-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="fc64c-682">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-683">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-683">**Old behavior**</span></span>

<span data-ttu-id="fc64c-684">В прошлом значения Guid хранились в SQLite в виде больших двоичных объектов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="fc64c-685">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-685">**New behavior**</span></span>

<span data-ttu-id="fc64c-686">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="fc64c-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="fc64c-687">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-687">**Why**</span></span>

<span data-ttu-id="fc64c-688">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="fc64c-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="fc64c-689">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="fc64c-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="fc64c-690">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-690">**Mitigations**</span></span>

<span data-ttu-id="fc64c-691">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="fc64c-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="fc64c-692">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="fc64c-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="fc64c-693">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="fc64c-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="fc64c-694">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="fc64c-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="fc64c-695">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="fc64c-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="fc64c-696">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-697">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-697">**Old behavior**</span></span>

<span data-ttu-id="fc64c-698">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="fc64c-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="fc64c-699">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="fc64c-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="fc64c-700">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-700">**New behavior**</span></span>

<span data-ttu-id="fc64c-701">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="fc64c-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="fc64c-702">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-702">**Why**</span></span>

<span data-ttu-id="fc64c-703">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="fc64c-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="fc64c-704">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-704">**Mitigations**</span></span>

<span data-ttu-id="fc64c-705">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="fc64c-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="fc64c-706">В EF Core можно и дальше использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="fc64c-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="fc64c-707">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="fc64c-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="fc64c-708">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="fc64c-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="fc64c-709">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="fc64c-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="fc64c-710">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-711">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-711">**Old behavior**</span></span>

<span data-ttu-id="fc64c-712">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="fc64c-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="fc64c-713">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-713">**New behavior**</span></span>

<span data-ttu-id="fc64c-714">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="fc64c-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="fc64c-715">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-715">**Why**</span></span>

<span data-ttu-id="fc64c-716">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="fc64c-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="fc64c-717">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="fc64c-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="fc64c-718">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-718">**Mitigations**</span></span>

<span data-ttu-id="fc64c-719">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="fc64c-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="fc64c-720">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="fc64c-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="fc64c-721">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="fc64c-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="fc64c-722">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="fc64c-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="fc64c-723">Удален метод UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="fc64c-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="fc64c-724">Отслеживание вопроса № 16400</span><span class="sxs-lookup"><span data-stu-id="fc64c-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="fc64c-725">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="fc64c-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fc64c-726">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-726">**Old behavior**</span></span>

<span data-ttu-id="fc64c-727">До выхода EF Core 3.0 `UseRowNumberForPaging` можно было использовать для создания кода SQL для разбиения на страницы, совместимого с SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="fc64c-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="fc64c-728">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-728">**New behavior**</span></span>

<span data-ttu-id="fc64c-729">Начиная с EF Core 3.0, EF будет формировать код SQL для разбиения на страницы, который совместим только с более поздними версиями SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fc64c-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="fc64c-730">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-730">**Why**</span></span>

<span data-ttu-id="fc64c-731">Причина для этого изменение в том, что [поддержка SQL Server 2008 прекращена](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/), а обновление этой функции для правильной работы с запросами, обновленными в EF Core 3.0, требует значительных трудозатрат.</span><span class="sxs-lookup"><span data-stu-id="fc64c-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="fc64c-732">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-732">**Mitigations**</span></span>

<span data-ttu-id="fc64c-733">Рекомендуется выполнить обновление до новой версии SQL Server или использовать более высокий уровень совместимости, чтобы обеспечить поддержку созданного кода SQL.</span><span class="sxs-lookup"><span data-stu-id="fc64c-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="fc64c-734">Если вы не можете это сделать, сообщите подробности в [соответствующей ветке](https://github.com/aspnet/EntityFrameworkCore/issues/16400).</span><span class="sxs-lookup"><span data-stu-id="fc64c-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="fc64c-735">Мы можем пересмотреть это решение, руководствуясь отзывами.</span><span class="sxs-lookup"><span data-stu-id="fc64c-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="fc64c-736">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="fc64c-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="fc64c-737">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="fc64c-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="fc64c-738">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="fc64c-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fc64c-739">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-739">**Old behavior**</span></span>

<span data-ttu-id="fc64c-740">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="fc64c-741">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-741">**New behavior**</span></span>

<span data-ttu-id="fc64c-742">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="fc64c-743">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-743">**Why**</span></span>

<span data-ttu-id="fc64c-744">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="fc64c-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="fc64c-745">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="fc64c-746">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-746">**Mitigations**</span></span>

<span data-ttu-id="fc64c-747">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="fc64c-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="fc64c-748">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="fc64c-749">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="fc64c-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="fc64c-750">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="fc64c-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="fc64c-751">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-752">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-752">**Change**</span></span>

<span data-ttu-id="fc64c-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="fc64c-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="fc64c-754">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-754">**Why**</span></span>

<span data-ttu-id="fc64c-755">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="fc64c-756">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-756">**Mitigations**</span></span>

<span data-ttu-id="fc64c-757">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="fc64c-757">Use the new name.</span></span> <span data-ttu-id="fc64c-758">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="fc64c-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="fc64c-759">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="fc64c-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="fc64c-760">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="fc64c-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="fc64c-761">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-762">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-762">**Old behavior**</span></span>

<span data-ttu-id="fc64c-763">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="fc64c-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="fc64c-764">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="fc64c-765">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-765">**New behavior**</span></span>

<span data-ttu-id="fc64c-766">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="fc64c-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="fc64c-767">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="fc64c-768">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-768">**Why**</span></span>

<span data-ttu-id="fc64c-769">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="fc64c-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="fc64c-770">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-770">**Mitigations**</span></span>

<span data-ttu-id="fc64c-771">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="fc64c-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="fc64c-772">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="fc64c-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="fc64c-773">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="fc64c-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="fc64c-774">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="fc64c-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fc64c-775">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-775">**Old behavior**</span></span>

<span data-ttu-id="fc64c-776">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="fc64c-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="fc64c-777">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-777">**New behavior**</span></span>

<span data-ttu-id="fc64c-778">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="fc64c-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="fc64c-779">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-779">**Why**</span></span>

<span data-ttu-id="fc64c-780">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="fc64c-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="fc64c-781">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="fc64c-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="fc64c-782">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-782">**Mitigations**</span></span>

<span data-ttu-id="fc64c-783">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="fc64c-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="fc64c-784">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="fc64c-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="fc64c-785">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="fc64c-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="fc64c-786">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="fc64c-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fc64c-787">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-787">**Old behavior**</span></span>

<span data-ttu-id="fc64c-788">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="fc64c-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="fc64c-789">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-789">**New behavior**</span></span>

<span data-ttu-id="fc64c-790">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="fc64c-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="fc64c-791">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="fc64c-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="fc64c-792">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-792">**Why**</span></span>

<span data-ttu-id="fc64c-793">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="fc64c-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="fc64c-794">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="fc64c-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="fc64c-795">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="fc64c-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="fc64c-796">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-796">**Mitigations**</span></span>

<span data-ttu-id="fc64c-797">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="fc64c-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="fc64c-798">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="fc64c-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="fc64c-799">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="fc64c-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="fc64c-800">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="fc64c-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="fc64c-801">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="fc64c-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fc64c-802">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-802">**Old behavior**</span></span>

<span data-ttu-id="fc64c-803">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="fc64c-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="fc64c-804">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-804">**New behavior**</span></span>

<span data-ttu-id="fc64c-805">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="fc64c-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="fc64c-806">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-806">**Why**</span></span>

<span data-ttu-id="fc64c-807">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc64c-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="fc64c-808">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="fc64c-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="fc64c-809">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-809">**Mitigations**</span></span>

<span data-ttu-id="fc64c-810">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="fc64c-811">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="fc64c-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="fc64c-812">Обновление NetTopologySuite до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="fc64c-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="fc64c-813">Отслеживание вопроса № 14825</span><span class="sxs-lookup"><span data-stu-id="fc64c-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="fc64c-814">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="fc64c-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fc64c-815">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-815">**Old behavior**</span></span>

<span data-ttu-id="fc64c-816">Пространственные пакеты раньше зависели от версии NetTopologySuite 1.15.1.</span><span class="sxs-lookup"><span data-stu-id="fc64c-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="fc64c-817">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-817">**New behavior**</span></span>

<span data-ttu-id="fc64c-818">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="fc64c-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="fc64c-819">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-819">**Why**</span></span>

<span data-ttu-id="fc64c-820">Версия NetTopologySuite 2.0.0 помогает решить ряд проблем с удобством использования, возникающих у пользователей EF Core.</span><span class="sxs-lookup"><span data-stu-id="fc64c-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="fc64c-821">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-821">**Mitigations**</span></span>

<span data-ttu-id="fc64c-822">NetTopologySuite версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="fc64c-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="fc64c-823">Подробные сведения см. в [заметках о выпуске](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="fc64c-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="fc64c-824">Множество неоднозначных связей со ссылкой на себя теперь требуют настройки</span><span class="sxs-lookup"><span data-stu-id="fc64c-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="fc64c-825">Отслеживание вопроса № 13573</span><span class="sxs-lookup"><span data-stu-id="fc64c-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="fc64c-826">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="fc64c-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fc64c-827">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-827">**Old behavior**</span></span>

<span data-ttu-id="fc64c-828">Тип сущности с множеством однонаправленных свойств навигации, ссылающихся на себя, и с соответствующими внешними ключами некорректно настраивался как единая связь.</span><span class="sxs-lookup"><span data-stu-id="fc64c-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="fc64c-829">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-829">For example:</span></span>

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

<span data-ttu-id="fc64c-830">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="fc64c-830">**New behavior**</span></span>

<span data-ttu-id="fc64c-831">Теперь этот сценарий обнаруживается при построении модели, и выдается исключение, указывающее, что модель неоднозначна.</span><span class="sxs-lookup"><span data-stu-id="fc64c-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="fc64c-832">**Причина**</span><span class="sxs-lookup"><span data-stu-id="fc64c-832">**Why**</span></span>

<span data-ttu-id="fc64c-833">Результирующая модель является неоднозначной и, скорее всего, будет неверной для этого случая.</span><span class="sxs-lookup"><span data-stu-id="fc64c-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="fc64c-834">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="fc64c-834">**Mitigations**</span></span>

<span data-ttu-id="fc64c-835">Выполняйте полную настройку связи.</span><span class="sxs-lookup"><span data-stu-id="fc64c-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="fc64c-836">Например:</span><span class="sxs-lookup"><span data-stu-id="fc64c-836">For example:</span></span>

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
