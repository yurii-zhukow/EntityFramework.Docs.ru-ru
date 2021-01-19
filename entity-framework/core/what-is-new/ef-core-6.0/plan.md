---
title: Планирование для Entity Framework Core 6.0
description: Темы и компоненты, запланированные для EF Core 6.0
author: ajcvickers
ms.date: 01/12/2021
uid: core/what-is-new/ef-core-6.0/plan
ms.openlocfilehash: 612461bc6ad30778baa5c6d10dda5cabac91dcb2
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129567"
---
# <a name="plan-for-entity-framework-core-60"></a><span data-ttu-id="b5f57-103">Планирование для Entity Framework Core 6.0</span><span class="sxs-lookup"><span data-stu-id="b5f57-103">Plan for Entity Framework Core 6.0</span></span>

<span data-ttu-id="b5f57-104">Как было описано в [процессе планирования](xref:core/what-is-new/release-planning), мы объединили полученные от заинтересованных лиц сведения в план для выпуска Entity Framework Core (EF Core) 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-104">As described in the [planning process](xref:core/what-is-new/release-planning), we have gathered input from stakeholders into a plan for the Entity Framework Core (EF Core) 6.0 release.</span></span>

<span data-ttu-id="b5f57-105">В отличие от предыдущих выпусков в этом плане мы не будем пытаться охватить весь объем работы, намеченный для выпуска 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-105">Unlike previous releases, this plan does not attempt to cover all work for the 6.0 release.</span></span> <span data-ttu-id="b5f57-106">В нем определяются направления и способы улучшения, предполагаемые в этом выпуске, а также предусмотрены возможности для внесения корректировок и добавления новых задач по мере получения отзывов и новых знаний в процессе работы над выпуском.</span><span class="sxs-lookup"><span data-stu-id="b5f57-106">Instead, it indicates where and how we intend to invest in this release, but with flexibility to adjust scope or pull in new work as we gather feedback and learn while working on the release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5f57-107">Этот план не устанавливает никаких обязательств.</span><span class="sxs-lookup"><span data-stu-id="b5f57-107">This plan is not a commitment.</span></span> <span data-ttu-id="b5f57-108">Он является отправной точкой и будет расширяться по мере получения новых знаний.</span><span class="sxs-lookup"><span data-stu-id="b5f57-108">It is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="b5f57-109">Могут быть включены некоторые вещи, пока не запланированные для версии 6.0,</span><span class="sxs-lookup"><span data-stu-id="b5f57-109">Some things not currently planned for 6.0 may get pulled in.</span></span> <span data-ttu-id="b5f57-110">и исключены некоторые вещи, уже запланированные для этой версии.</span><span class="sxs-lookup"><span data-stu-id="b5f57-110">Some things currently planned for 6.0 may get punted out.</span></span>

## <a name="general-information"></a><span data-ttu-id="b5f57-111">Общие сведения</span><span class="sxs-lookup"><span data-stu-id="b5f57-111">General information</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="b5f57-112">Номер версии и дата выпуска</span><span class="sxs-lookup"><span data-stu-id="b5f57-112">Version number and release date</span></span>

<span data-ttu-id="b5f57-113">Выпуск EF Core 6.0 будет следовать после EF Core 5.0 и на данный момент запланирован на ноябрь 2021 года в одно время с .NET 6.</span><span class="sxs-lookup"><span data-stu-id="b5f57-113">EF Core 6.0 is the next release after EF Core 5.0 and is currently scheduled for release in November 2021 at the same time as .NET 6.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="b5f57-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="b5f57-114">Supported platforms</span></span>

<span data-ttu-id="b5f57-115">EF Core 6.0 на данный момент предназначается для работы с платформой .NET 5.</span><span class="sxs-lookup"><span data-stu-id="b5f57-115">EF Core 6.0 currently targets .NET 5.</span></span> <span data-ttu-id="b5f57-116">Ближе к моменту выпуска целевая платформа, скорее всего, будет изменена на .NET 6.</span><span class="sxs-lookup"><span data-stu-id="b5f57-116">This will likely be updated to .NET 6 as we near the release.</span></span> <span data-ttu-id="b5f57-117">EF Core 6.0 не предназначается для работы с какой-либо версией .NET Standard. Дополнительные сведения см. в разделе [Перспективы .NET Standard](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard/).</span><span class="sxs-lookup"><span data-stu-id="b5f57-117">EF Core 6.0 does not target any .NET Standard version; for more information see [the future of .NET Standard](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard/).</span></span>

<span data-ttu-id="b5f57-118">EF Core 6.0 не предназначается для работы с .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b5f57-118">EF Core 6.0 will not run on .NET Framework.</span></span>

<span data-ttu-id="b5f57-119">EF Core 6.0 будет согласовываться с .NET 6 в качестве [выпуска с долгосрочной поддержкой (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="b5f57-119">EF Core 6.0 will align with .NET 6 as a [long-term support (LTS) release](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="b5f57-120">Критические изменения</span><span class="sxs-lookup"><span data-stu-id="b5f57-120">Breaking changes</span></span>

<span data-ttu-id="b5f57-121">EF Core 6.0 будет содержать несколько [критических изменений](xref:core/what-is-new/ef-core-6.0/breaking-changes), которые планируются к реализации в рамках процесса развития EF Core и платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="b5f57-121">EF Core 6.0 will contain a small number of [breaking changes](xref:core/what-is-new/ef-core-6.0/breaking-changes) as we continue to evolve both EF Core and the .NET platform.</span></span> <span data-ttu-id="b5f57-122">Наша цель состоит в том, чтобы позволить подавляющему большинству приложений осуществлять обновление без нарушения работы.</span><span class="sxs-lookup"><span data-stu-id="b5f57-122">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

## <a name="themes"></a><span data-ttu-id="b5f57-123">Темы</span><span class="sxs-lookup"><span data-stu-id="b5f57-123">Themes</span></span>

<span data-ttu-id="b5f57-124">Ниже перечислены основные направления, по которым планируется работа над выпуском EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-124">The following areas will form the basis for the large investments in EF Core 6.0.</span></span>

## <a name="highly-requested-features"></a><span data-ttu-id="b5f57-125">Самые востребованные возможности</span><span class="sxs-lookup"><span data-stu-id="b5f57-125">Highly requested features</span></span>

<span data-ttu-id="b5f57-126">Традиционно основным источником данных для [процесса планирования](xref:core/what-is-new/release-planning) являются ваши [голоса (👍), отдаваемые за новые возможности на сайте GitHub](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="b5f57-126">As always, a major input into the [planning process](xref:core/what-is-new/release-planning) comes from the [voting (👍) for features on GitHub](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc).</span></span> <span data-ttu-id="b5f57-127">В рамках выпуска EF Core 6.0 планируется работа над следующими самыми востребованными возможностями.</span><span class="sxs-lookup"><span data-stu-id="b5f57-127">For EF Core 6.0 we plan to work on the following highly requested features:</span></span>

### <a name="sql-server-temporal-tables"></a><span data-ttu-id="b5f57-128">Временные таблицы SQL Server</span><span class="sxs-lookup"><span data-stu-id="b5f57-128">SQL Server temporal tables</span></span>

<span data-ttu-id="b5f57-129">Отслеживается по проблеме [№ 4693](https://github.com/dotnet/efcore/issues/4693)</span><span class="sxs-lookup"><span data-stu-id="b5f57-129">Tracked by [#4693](https://github.com/dotnet/efcore/issues/4693)</span></span>

<span data-ttu-id="b5f57-130">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-130">Status: Not started</span></span>

<span data-ttu-id="b5f57-131">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-131">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-132">Временные таблицы поддерживают запросы к данным, которые хранились в них _любой момент времени_, а не только к самым последним сохраненным данным, как в случае с обычными таблицами.</span><span class="sxs-lookup"><span data-stu-id="b5f57-132">Temporal tables support queries for data stored in the table at _any point in time_, as opposed to only the most recent data stored as is the case for normal tables.</span></span> <span data-ttu-id="b5f57-133">EF Core 6.0 поддерживает создание временных таблиц с помощью миграций, а также доступ к данным посредством запросов LINQ.</span><span class="sxs-lookup"><span data-stu-id="b5f57-133">EF Core 6.0 will allow temporal tables to be created via Migrations, as well as allowing access to the data through LINQ queries.</span></span>

<span data-ttu-id="b5f57-134">Начальный объем работы в этом направлении приводится в [описании этой проблемы](https://github.com/dotnet/efcore/issues/4693#issuecomment-625048974).</span><span class="sxs-lookup"><span data-stu-id="b5f57-134">This work is initially scoped as [described on the issue](https://github.com/dotnet/efcore/issues/4693#issuecomment-625048974).</span></span> <span data-ttu-id="b5f57-135">Возможно, с учетом полученных в процессе работы над выпуском отзывов будет реализована дополнительная поддержка.</span><span class="sxs-lookup"><span data-stu-id="b5f57-135">We may pull in additional support based on feedback during the release.</span></span>

### <a name="json-columns"></a><span data-ttu-id="b5f57-136">Столбцы JSON</span><span class="sxs-lookup"><span data-stu-id="b5f57-136">JSON columns</span></span>

<span data-ttu-id="b5f57-137">Отслеживается по проблеме [№ 4021](https://github.com/dotnet/efcore/issues/4021)</span><span class="sxs-lookup"><span data-stu-id="b5f57-137">Tracked by [#4021](https://github.com/dotnet/efcore/issues/4021)</span></span>

<span data-ttu-id="b5f57-138">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-138">Status: Not started</span></span>

<span data-ttu-id="b5f57-139">Размер футболки: Средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-139">T-shirt size: Medium</span></span>

<span data-ttu-id="b5f57-140">Эта возможность позволит определить общие механизмы и шаблоны для поддержки JSON, которые могут быть реализованы любым поставщиком базы данных.</span><span class="sxs-lookup"><span data-stu-id="b5f57-140">This feature will introduce a common mechanism and patterns for JSON support that can be implemented by any database provider.</span></span> <span data-ttu-id="b5f57-141">Мы планируем совместно с участниками сообщества согласовать существующие реализации для [Npgsql](https://github.com/npgsql/efcore.pg) и [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), а также добавить поддержку для SQL Server и SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5f57-141">We will work with the community to align existing implementations for [Npgsql](https://github.com/npgsql/efcore.pg) and [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and also add support for SQL Server and SQLite.</span></span>

### <a name="columnattributeorder"></a><span data-ttu-id="b5f57-142">ColumnAttribute.Order</span><span class="sxs-lookup"><span data-stu-id="b5f57-142">ColumnAttribute.Order</span></span>

<span data-ttu-id="b5f57-143">Отслеживается по проблеме [№ 10059](https://github.com/dotnet/efcore/issues/10059)</span><span class="sxs-lookup"><span data-stu-id="b5f57-143">Tracked by [#10059](https://github.com/dotnet/efcore/issues/10059)</span></span>

<span data-ttu-id="b5f57-144">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-144">Status: Not started</span></span>

<span data-ttu-id="b5f57-145">Размер футболки: маленький</span><span class="sxs-lookup"><span data-stu-id="b5f57-145">T-shirt size: Small</span></span>

<span data-ttu-id="b5f57-146">Эта возможность позволяет использовать произвольный порядок столбцов при **создании таблицы** с помощью миграций или `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="b5f57-146">This feature will allow arbitrary ordering of columns when **creating a table** with Migrations or `EnsureCreated`.</span></span> <span data-ttu-id="b5f57-147">Обратите внимание, что для изменения порядка столбцов в существующих таблицах требуется перестроение таблицы. Поддержка такой возможности в каких-либо выпусках EF Core не планируется.</span><span class="sxs-lookup"><span data-stu-id="b5f57-147">Note that changing the order of columns in an existing tables requires that the table be rebuilt, and this is not something that we plan to support in any EF Core release.</span></span>

## <a name="performance"></a><span data-ttu-id="b5f57-148">Производительность</span><span class="sxs-lookup"><span data-stu-id="b5f57-148">Performance</span></span>

<span data-ttu-id="b5f57-149">В целом EF Core работает быстрее, чем EF6, но при этом все же остаются направления, в которых возможны значительные улучшения производительности.</span><span class="sxs-lookup"><span data-stu-id="b5f57-149">While EF Core is generally faster than EF6, there are still areas where significant improvements in performance are possible.</span></span> <span data-ttu-id="b5f57-150">В выпуске EF Core 6.0 мы планируем решить ряд соответствующих задач, а также добиться улучшения нашей инфраструктуры и производительности и тестов.</span><span class="sxs-lookup"><span data-stu-id="b5f57-150">We plan to tackle several of these areas in EF Core 6.0, while also improving our perf infrastructure and testing.</span></span>

<span data-ttu-id="b5f57-151">Изучение этой темы требует большого объема циклических исследований, по результатам которых мы будем определять направления для концентрации основных усилий.</span><span class="sxs-lookup"><span data-stu-id="b5f57-151">This theme will involve a lot of iterative investigation, which will inform where we focus resources.</span></span> <span data-ttu-id="b5f57-152">Мы планируем начать с решения следующих задач.</span><span class="sxs-lookup"><span data-stu-id="b5f57-152">We plan to begin with:</span></span>

### <a name="performance-infrastructure-and-new-tests"></a><span data-ttu-id="b5f57-153">Инфраструктура производительности и новые тесты</span><span class="sxs-lookup"><span data-stu-id="b5f57-153">Performance infrastructure and new tests</span></span>

<span data-ttu-id="b5f57-154">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-154">Status: Not started</span></span>

<span data-ttu-id="b5f57-155">Размер футболки: Средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-155">T-shirt size: Medium</span></span>

<span data-ttu-id="b5f57-156">В базе кода EF Core уже реализован набор тестов производительности, выполняемых на ежедневной основе.</span><span class="sxs-lookup"><span data-stu-id="b5f57-156">The EF Core codebase already contains a set of performance benchmarks that are executed every day.</span></span> <span data-ttu-id="b5f57-157">В выпуске 6.0 мы планируем усовершенствовать инфраструктуру для этих тестов и добавить новые.</span><span class="sxs-lookup"><span data-stu-id="b5f57-157">For 6.0, we plan to improve the infrastructure for these tests as well as adding new tests.</span></span> <span data-ttu-id="b5f57-158">Мы также планируем определить профили основных сценариев, связанных с производительностью, и исправить самые очевидные проблемы.</span><span class="sxs-lookup"><span data-stu-id="b5f57-158">We will also profile mainline perf scenarios and fix any low-hanging fruit found.</span></span>

### <a name="compiled-models"></a><span data-ttu-id="b5f57-159">Скомпилированные модели</span><span class="sxs-lookup"><span data-stu-id="b5f57-159">Compiled models</span></span>

<span data-ttu-id="b5f57-160">Отслеживается по проблеме [№ 1906](https://github.com/dotnet/efcore/issues/1906)</span><span class="sxs-lookup"><span data-stu-id="b5f57-160">Tracked by [#1906](https://github.com/dotnet/efcore/issues/1906)</span></span>

<span data-ttu-id="b5f57-161">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-161">Status: Not started</span></span>

<span data-ttu-id="b5f57-162">Размер футболки: очень большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-162">T-shirt size: X-Large</span></span>

<span data-ttu-id="b5f57-163">Благодаря скомпилированным моделям станет возможным создание скомпилированной формы модели EF.</span><span class="sxs-lookup"><span data-stu-id="b5f57-163">Compiled models will allow the generation of a compiled form of the EF model.</span></span> <span data-ttu-id="b5f57-164">Это позволит повысить высокую производительность при запуске, а также общую производительность доступа к модели.</span><span class="sxs-lookup"><span data-stu-id="b5f57-164">This will provide both better startup performance, as well as generally better performance when accessing the model.</span></span>

### <a name="techempower-fortunes"></a><span data-ttu-id="b5f57-165">Тесты TechEmpower Fortunes</span><span class="sxs-lookup"><span data-stu-id="b5f57-165">TechEmpower Fortunes</span></span>

<span data-ttu-id="b5f57-166">Отслеживается по проблеме [№ 23611](https://github.com/dotnet/efcore/issues/23611)</span><span class="sxs-lookup"><span data-stu-id="b5f57-166">Tracked by [#23611](https://github.com/dotnet/efcore/issues/23611)</span></span>

<span data-ttu-id="b5f57-167">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-167">Status: Not started</span></span>

<span data-ttu-id="b5f57-168">Размер футболки: очень большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-168">T-shirt size: X-Large</span></span>

<span data-ttu-id="b5f57-169">На протяжении нескольких лет мы проводим соответствующие отраслевым стандартам [тесты производительности TechEmpower](https://www.techempower.com/benchmarks/) для .NET в отношении базы данных PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b5f57-169">We have been running the industry standard [TechEmpower benchmarks](https://www.techempower.com/benchmarks/) on .NET against a PostgreSQL database for several years.</span></span> <span data-ttu-id="b5f57-170">Для сценариев EF в наибольшей степени подходит [тест Fortunes](https://www.techempower.com/benchmarks/#section=data-r19&hw=ph&test=fortune).</span><span class="sxs-lookup"><span data-stu-id="b5f57-170">The [Fortunes benchmark](https://www.techempower.com/benchmarks/#section=data-r19&hw=ph&test=fortune) is particularly relevant to EF scenarios.</span></span> <span data-ttu-id="b5f57-171">Мы применяем несколько разновидностей этого теста, включая следующие.</span><span class="sxs-lookup"><span data-stu-id="b5f57-171">We have multiple variations of this benchmark, including:</span></span>

- <span data-ttu-id="b5f57-172">Реализация, в которой напрямую используется ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="b5f57-172">An implementation that uses ADO.NET directly.</span></span> <span data-ttu-id="b5f57-173">Эта реализация является самой быстрой из трех перечисленных здесь.</span><span class="sxs-lookup"><span data-stu-id="b5f57-173">This is the fastest implementation of the three listed here.</span></span>
- <span data-ttu-id="b5f57-174">Реализация, в которой используется [Dapper](https://www.nuget.org/packages/Dapper/).</span><span class="sxs-lookup"><span data-stu-id="b5f57-174">An implementation that uses [Dapper](https://www.nuget.org/packages/Dapper/).</span></span> <span data-ttu-id="b5f57-175">Она работает медленнее, чем реализация с прямым использованием ADO.NET, но по-прежнему обеспечивает достаточно высокую скорость.</span><span class="sxs-lookup"><span data-stu-id="b5f57-175">This is slower than using ADO.NET directly, but still fast.</span></span>
- <span data-ttu-id="b5f57-176">Реализация, в которой используется EF Core.</span><span class="sxs-lookup"><span data-stu-id="b5f57-176">An implementation that uses EF Core.</span></span> <span data-ttu-id="b5f57-177">В настоящее время это самая медленная реализация из трех представленных.</span><span class="sxs-lookup"><span data-stu-id="b5f57-177">This is currently the slowest implementation of the three.</span></span>

<span data-ttu-id="b5f57-178">В выпуске EF Core 6.0 мы постараемся добиться для EF Core показателей производительности, соответствующих реализации с Dapper, на основе результатов теста TechEmpower Fortunes.</span><span class="sxs-lookup"><span data-stu-id="b5f57-178">The goal for EF Core 6.0 is to get the EF Core performance to match that of Dapper on the TechEmpower Fortunes benchmark.</span></span> <span data-ttu-id="b5f57-179">(Это сложная задача, но мы приложим все силы, чтобы достичь лучшего возможного результата.)</span><span class="sxs-lookup"><span data-stu-id="b5f57-179">(This is a significant challenge but we will do our best to get as close as we can.)</span></span>

### <a name="linkeraot"></a><span data-ttu-id="b5f57-180">Компоновщик или компиляция AOT</span><span class="sxs-lookup"><span data-stu-id="b5f57-180">Linker/AOT</span></span>

<span data-ttu-id="b5f57-181">Отслеживается по проблеме [№ 10963](https://github.com/dotnet/efcore/issues/10963)</span><span class="sxs-lookup"><span data-stu-id="b5f57-181">Tracked by [#10963](https://github.com/dotnet/efcore/issues/10963)</span></span>

<span data-ttu-id="b5f57-182">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-182">Status: Not started</span></span>

<span data-ttu-id="b5f57-183">Размер футболки: Средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-183">T-shirt size: Medium</span></span>

<span data-ttu-id="b5f57-184">EF Core создает большой объем кода во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="b5f57-184">EF Core performs large amounts of runtime code generation.</span></span> <span data-ttu-id="b5f57-185">Это может стать проблемой для моделей приложений, которые зависят от встряхивания дерева компоновщика, таких как Xamarin и Blazor, а также для платформ, не допускающих динамическую компиляцию, таких как iOS.</span><span class="sxs-lookup"><span data-stu-id="b5f57-185">This is challenging for app models that depend on linker tree shaking, such as Xamarin and Blazor, and platforms that don't allow dynamic compilation, such as iOS.</span></span> <span data-ttu-id="b5f57-186">В процессе работы над выпуском EF Core 6.0 мы продолжим исследования в этом направлении и будем по мере возможности вносить точечные улучшения.</span><span class="sxs-lookup"><span data-stu-id="b5f57-186">We will continue investigating in this space as part of EF Core 6.0 and make targeted improvements as we can.</span></span> <span data-ttu-id="b5f57-187">Тем не менее мы не рассчитываем полностью решить эту проблему в рамках выпуска 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-187">However, we do not expect to fully close the gap in the 6.0 time frame.</span></span>

## <a name="migrations-and-deployment"></a><span data-ttu-id="b5f57-188">Миграция и развертывание</span><span class="sxs-lookup"><span data-stu-id="b5f57-188">Migrations and deployment</span></span>

<span data-ttu-id="b5f57-189">По результатам [исследований, проведенных при работе над выпуском EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan#migrations-and-deployment-experience), мы планируем реализовать улучшенную поддержку управления миграцией и развертыванием баз данных.</span><span class="sxs-lookup"><span data-stu-id="b5f57-189">Following on from the [investigations done for EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan#migrations-and-deployment-experience), we plan to introduce improved support for managing migrations and deploying databases.</span></span> <span data-ttu-id="b5f57-190">Предполагаются улучшения по двум основным направлениям.</span><span class="sxs-lookup"><span data-stu-id="b5f57-190">This includes two major areas:</span></span>

### <a name="migrations-bundles"></a><span data-ttu-id="b5f57-191">Пакеты миграций</span><span class="sxs-lookup"><span data-stu-id="b5f57-191">Migrations bundles</span></span>

<span data-ttu-id="b5f57-192">Отслеживается по проблеме [№ 19693](https://github.com/dotnet/efcore/issues/19693)</span><span class="sxs-lookup"><span data-stu-id="b5f57-192">Tracked by [#19693](https://github.com/dotnet/efcore/issues/19693)</span></span>

<span data-ttu-id="b5f57-193">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-193">Status: Not started</span></span>

<span data-ttu-id="b5f57-194">Размер футболки: Средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-194">T-shirt size: Medium</span></span>

<span data-ttu-id="b5f57-195">Пакет миграций — это автономный исполняемый файл, который применяет миграции к рабочей базе данных.</span><span class="sxs-lookup"><span data-stu-id="b5f57-195">A migrations bundle is a self-contained executable that applies migrations to a production database.</span></span> <span data-ttu-id="b5f57-196">Он действует аналогично команде `dotnet ef database update`, но при этом должен значительно упростить развертывание SSH, DOCKER или PowerShell за счет включения всех необходимых компонентов в один исполняемый файл.</span><span class="sxs-lookup"><span data-stu-id="b5f57-196">The behavior will match `dotnet ef database update`, but should make SSH/Docker/PowerShell deployment much easier, since everything needed is contained in the single executable.</span></span>

### <a name="managing-migrations"></a><span data-ttu-id="b5f57-197">Управление миграциями</span><span class="sxs-lookup"><span data-stu-id="b5f57-197">Managing migrations</span></span>

<span data-ttu-id="b5f57-198">Отслеживается по проблеме [№ 22945](https://github.com/dotnet/efcore/issues/22945)</span><span class="sxs-lookup"><span data-stu-id="b5f57-198">Tracked by [#22945](https://github.com/dotnet/efcore/issues/22945)</span></span>

<span data-ttu-id="b5f57-199">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-199">Status: Not started</span></span>

<span data-ttu-id="b5f57-200">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-200">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-201">Увеличение количества миграций, создаваемых для приложения, может стать проблемой.</span><span class="sxs-lookup"><span data-stu-id="b5f57-201">The number of migrations created for an application can grow to become a burden.</span></span> <span data-ttu-id="b5f57-202">Кроме того, такие миграции часто развертываются вместе с приложением, даже если это не требуется.</span><span class="sxs-lookup"><span data-stu-id="b5f57-202">In addition, these migrations are frequently deployed with the application even when this is not needed.</span></span> <span data-ttu-id="b5f57-203">В связи с этим в EF Core 6.0 мы планируем реализовать улучшенные средства, а также функции управления проектами и сборками.</span><span class="sxs-lookup"><span data-stu-id="b5f57-203">In EF Core 6.0, we plan to improve this through better tooling and project/assembly management.</span></span> <span data-ttu-id="b5f57-204">В частности, мы планируем решить эти две проблемы: [сжатие нескольких миграций в одну](https://github.com/dotnet/efcore/issues/2174) и [повторное создание чистого моментального снимка модели](https://github.com/dotnet/efcore/issues/18557).</span><span class="sxs-lookup"><span data-stu-id="b5f57-204">Two specific issues we plan to address are [squash many migrations into one](https://github.com/dotnet/efcore/issues/2174) and [regenerate a clean model snapshot](https://github.com/dotnet/efcore/issues/18557).</span></span>

## <a name="improve-existing-features-and-fix-bugs"></a><span data-ttu-id="b5f57-205">Улучшение существующих возможностей и исправление ошибок</span><span class="sxs-lookup"><span data-stu-id="b5f57-205">Improve existing features and fix bugs</span></span>

<span data-ttu-id="b5f57-206">На данный момент в этом выпуске планируется решение всех [проблем или ошибок, назначенных для вехи 6.0.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0).</span><span class="sxs-lookup"><span data-stu-id="b5f57-206">Any [issue or bug assigned to the 6.0.0 milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0) is currently planned for this release.</span></span> <span data-ttu-id="b5f57-207">К ним относится множество небольших улучшений и исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="b5f57-207">This includes many small enhancements and bug fixes.</span></span>

### <a name="ef6-query-parity"></a><span data-ttu-id="b5f57-208">Соответствие запросов в EF6</span><span class="sxs-lookup"><span data-stu-id="b5f57-208">EF6 query parity</span></span>

<span data-ttu-id="b5f57-209">Отслеживается с помощью [проблем с меткой ef6-parity в вехе 6.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+label%3Aef6-parity+milestone%3A6.0.0)</span><span class="sxs-lookup"><span data-stu-id="b5f57-209">Tracked by [issues labeled with 'ef6-parity' and in the 6.0 milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+label%3Aef6-parity+milestone%3A6.0.0)</span></span>

<span data-ttu-id="b5f57-210">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-210">Status: Not started</span></span>

<span data-ttu-id="b5f57-211">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-211">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-212">EF Core 5.0 поддерживает большинство шаблонов запросов, поддерживаемых в EF6, а также шаблоны, которые не поддерживаются в EF6.</span><span class="sxs-lookup"><span data-stu-id="b5f57-212">EF Core 5.0 supports most query patterns supported by EF6, in addition to patterns not supported in EF6.</span></span> <span data-ttu-id="b5f57-213">В выпуске EF Core 6.0 мы планируем устранить это несоответствие и включить в число поддерживаемых EF Core запросов все запросы EF6.</span><span class="sxs-lookup"><span data-stu-id="b5f57-213">For EF Core 6.0, we plan to close the gap and make supported EF Core queries a true superset of supported EF6 queries.</span></span> <span data-ttu-id="b5f57-214">Для этого будет проведен анализ несоответствий. Тем не менее уже сейчас определены проблемы с запросом GroupBy, например [с преобразованием запроса GroupBy с последующим запросом FirstOrDefault](https://github.com/dotnet/efcore/issues/12088), и необработанными запросами SQL для [примитивных](https://github.com/dotnet/efcore/issues/11624) и [несопоставленных](https://github.com/dotnet/efcore/issues/10753) типов.</span><span class="sxs-lookup"><span data-stu-id="b5f57-214">This will be driven by investigation of the gaps, but already includes GroupBy issues such as [translate GroupBy followed by FirstOrDefault](https://github.com/dotnet/efcore/issues/12088), and raw SQL queries for [primitive](https://github.com/dotnet/efcore/issues/11624) and [unmapped](https://github.com/dotnet/efcore/issues/10753) types.</span></span>  

### <a name="value-objects"></a><span data-ttu-id="b5f57-215">Объекты значений</span><span class="sxs-lookup"><span data-stu-id="b5f57-215">Value objects</span></span>

<span data-ttu-id="b5f57-216">Отслеживается по проблеме [№ 9906](https://github.com/dotnet/efcore/issues/9906)</span><span class="sxs-lookup"><span data-stu-id="b5f57-216">Tracked by [#9906](https://github.com/dotnet/efcore/issues/9906)</span></span>

<span data-ttu-id="b5f57-217">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-217">Status: Not started</span></span>

<span data-ttu-id="b5f57-218">Размер футболки: Средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-218">T-shirt size: Medium</span></span>

<span data-ttu-id="b5f57-219">Ранее наша команда разработчиков полагала, что принадлежащие сущности, которые предназначены для [поддержки статистических вычислений](https://www.martinfowler.com/bliki/DDD_Aggregate.html), также целесообразно использовать для аппроксимации [объектов значений](https://www.martinfowler.com/bliki/ValueObject.html).</span><span class="sxs-lookup"><span data-stu-id="b5f57-219">It was previously the team view that owned entities, intended for [aggregate support](https://www.martinfowler.com/bliki/DDD_Aggregate.html), would also be a reasonable approximation to [value objects](https://www.martinfowler.com/bliki/ValueObject.html).</span></span> <span data-ttu-id="b5f57-220">Однако опыт показал, что это не так.</span><span class="sxs-lookup"><span data-stu-id="b5f57-220">Experience has shown this not to be the case.</span></span> <span data-ttu-id="b5f57-221">Соответственно, в EF Core 6.0 мы планируем оптимизировать использование объектов значений в рамках проблемно-ориентированного проектирования.</span><span class="sxs-lookup"><span data-stu-id="b5f57-221">Therefore, in EF Core 6.0 we plan to introduce a better experience focused on the needs of value objects in domain-driven design.</span></span> <span data-ttu-id="b5f57-222">Этот подход будет основываться на преобразователях значений, а не на принадлежащих сущностях.</span><span class="sxs-lookup"><span data-stu-id="b5f57-222">This approach will be based on value converters rather than owned entities.</span></span>

<span data-ttu-id="b5f57-223">Изначально объем работы в этом направлении ограничен поддержкой [преобразователей значений, которые сопоставляются с несколькими столбцами](https://github.com/dotnet/efcore/issues/13947).</span><span class="sxs-lookup"><span data-stu-id="b5f57-223">This work is initially scoped to allow [value converters which map to multiple columns](https://github.com/dotnet/efcore/issues/13947).</span></span> <span data-ttu-id="b5f57-224">Возможно, с учетом полученных в процессе работы над выпуском отзывов будет реализована дополнительная поддержка.</span><span class="sxs-lookup"><span data-stu-id="b5f57-224">We may pull in additional support based on feedback during the release.</span></span>

### <a name="cosmos-database-provider"></a><span data-ttu-id="b5f57-225">Поставщик базы данных Cosmos</span><span class="sxs-lookup"><span data-stu-id="b5f57-225">Cosmos database provider</span></span>

<span data-ttu-id="b5f57-226">Отслеживается с помощью [проблем с меткой area-cosmos в вехе 6.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0+label%3Aarea-cosmos)</span><span class="sxs-lookup"><span data-stu-id="b5f57-226">Tracked by [issues labeled with 'area-cosmos' and in the 6.0 milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0+label%3Aarea-cosmos)</span></span>

<span data-ttu-id="b5f57-227">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-227">Status: Not started</span></span>

<span data-ttu-id="b5f57-228">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-228">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-229">Мы активно собираем отзывы о том, какие улучшения необходимо внести в поставщик Cosmos в EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-229">We are actively gathering feedback on which improvements to make to the Cosmos provider in EF Core 6.0.</span></span> <span data-ttu-id="b5f57-230">По мере получения новой информации мы будем обновлять этот документ.</span><span class="sxs-lookup"><span data-stu-id="b5f57-230">We will update this document as we learn more.</span></span> <span data-ttu-id="b5f57-231">На данный момент мы ждем ваших голосов (👍) за необходимые функции Cosmos.</span><span class="sxs-lookup"><span data-stu-id="b5f57-231">For now, please make sure to vote (👍) for the Cosmos features that you need.</span></span>

### <a name="expose-model-building-conventions-to-applications"></a><span data-ttu-id="b5f57-232">Предоставление приложениям доступа к соглашениям о построении модели</span><span class="sxs-lookup"><span data-stu-id="b5f57-232">Expose model building conventions to applications</span></span>

<span data-ttu-id="b5f57-233">Отслеживается по проблеме [№ 214](https://github.com/dotnet/efcore/issues/214)</span><span class="sxs-lookup"><span data-stu-id="b5f57-233">Tracked by [#214](https://github.com/dotnet/efcore/issues/214)</span></span>

<span data-ttu-id="b5f57-234">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-234">Status: Not started</span></span>

<span data-ttu-id="b5f57-235">Размер футболки: средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-235">T-shirt size: medium</span></span>

<span data-ttu-id="b5f57-236">В EF Core используется набор соглашений о построении модели на основе типов .NET.</span><span class="sxs-lookup"><span data-stu-id="b5f57-236">EF Core uses a set of conventions for building a model from .NET types.</span></span> <span data-ttu-id="b5f57-237">В настоящее время эти соглашения определяются поставщиком базы данных.</span><span class="sxs-lookup"><span data-stu-id="b5f57-237">These conventions are currently controlled by the database provider.</span></span> <span data-ttu-id="b5f57-238">В EF Core 6.0 мы планируем предоставить приложениям возможность получать доступ к этим соглашениям и изменять их.</span><span class="sxs-lookup"><span data-stu-id="b5f57-238">In EF Core 6.0, we intend to allow applications to hook into and change these conventions.</span></span>

### <a name="zero-bug-balance-zbb"></a><span data-ttu-id="b5f57-239">Отсутствие ошибок</span><span class="sxs-lookup"><span data-stu-id="b5f57-239">Zero bug balance (ZBB)</span></span>

<span data-ttu-id="b5f57-240">Отслеживается с помощью [проблем с меткой `type-bug` в вехе 6.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A6.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="b5f57-240">Tracked by [issues labeled with `type-bug` in the 6.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A6.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="b5f57-241">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="b5f57-241">Status: In-progress</span></span>

<span data-ttu-id="b5f57-242">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-242">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-243">Мы планируем устранить все нерешенные ошибки во время работы над выпуском EF Core 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-243">We plan to fix all outstanding bugs during the EF Core 6.0 time frame.</span></span> <span data-ttu-id="b5f57-244">Важные аспекты</span><span class="sxs-lookup"><span data-stu-id="b5f57-244">Some things to keep in mind:</span></span>

- <span data-ttu-id="b5f57-245">Это относится к проблемам с меткой [type-bug](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Atype-bug).</span><span class="sxs-lookup"><span data-stu-id="b5f57-245">This specifically applies to issues labeled [type-bug](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Atype-bug).</span></span>
- <span data-ttu-id="b5f57-246">Будут определены исключения, например ошибки, для устранения которых требуется изменение проекта или реализация новой функции.</span><span class="sxs-lookup"><span data-stu-id="b5f57-246">There will be exceptions, such as when the bug requires a design change or new feature to fix properly.</span></span> <span data-ttu-id="b5f57-247">Такие проблемы получат метку `blocked`.</span><span class="sxs-lookup"><span data-stu-id="b5f57-247">These issues will be marked with the `blocked` label.</span></span>
- <span data-ttu-id="b5f57-248">При необходимости по мере приближения выхода общедоступной версии или RTM-выпуска мы будем прекращать работу над некоторыми ошибками с учетом связанной с ними степени риска.</span><span class="sxs-lookup"><span data-stu-id="b5f57-248">We will punt bugs based on risk when needed as is normal as we get close to a GA/RTM release.</span></span>

### <a name="miscellaneous-features"></a><span data-ttu-id="b5f57-249">Прочие функции</span><span class="sxs-lookup"><span data-stu-id="b5f57-249">Miscellaneous features</span></span>

<span data-ttu-id="b5f57-250">Отслеживается с помощью [проблем с меткой `type-enhancement` в вехе 6.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0+label%3Atype-enhancement)</span><span class="sxs-lookup"><span data-stu-id="b5f57-250">Tracked by [issues labeled with `type-enhancement` in the 6.0 milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0+label%3Atype-enhancement)</span></span>

<span data-ttu-id="b5f57-251">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="b5f57-251">Status: In-progress</span></span>

<span data-ttu-id="b5f57-252">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-252">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-253">В EF 6.0 планируется включить ряд прочих функций, в том числе следующие.</span><span class="sxs-lookup"><span data-stu-id="b5f57-253">Miscellaneous features planned for EF 6.0 include, but are not limited to:</span></span>

- [<span data-ttu-id="b5f57-254">Разделенные запросы для коллекций, не связанных с навигацией</span><span class="sxs-lookup"><span data-stu-id="b5f57-254">Split query for non-navigation collections</span></span>](https://github.com/dotnet/efcore/issues/21234)
- [<span data-ttu-id="b5f57-255">Обнаружение простых таблиц соединения при реконструировании и создание связей "многие ко многим"</span><span class="sxs-lookup"><span data-stu-id="b5f57-255">Detect simple join tables in reverse engineering and create many-to-many relationships</span></span>](https://github.com/dotnet/efcore/issues/22475)
- [<span data-ttu-id="b5f57-256">Полнотекстовый или свободный поиск в SQLite и SQL Server</span><span class="sxs-lookup"><span data-stu-id="b5f57-256">Complete full/free-text search on SQLite and SQL Server</span></span>](https://github.com/dotnet/efcore/issues/4823)
- [<span data-ttu-id="b5f57-257">Пространственные индексы SQL Server</span><span class="sxs-lookup"><span data-stu-id="b5f57-257">SQL Server spatial indexes</span></span>](https://github.com/dotnet/efcore/issues/12538)
- [<span data-ttu-id="b5f57-258">Механизм или API, позволяющий задавать преобразование по умолчанию для любого свойства конкретного типа в модели</span><span class="sxs-lookup"><span data-stu-id="b5f57-258">Mechanism/API to specify a default conversion for any property of a given type in the model</span></span>](https://github.com/dotnet/efcore/issues/10784)
- [<span data-ttu-id="b5f57-259">Использование нового API пакетной обработки из ADO.NET</span><span class="sxs-lookup"><span data-stu-id="b5f57-259">Use the new batching API from ADO.NET</span></span>](https://github.com/dotnet/efcore/issues/18990)

## <a name="net-integration"></a><span data-ttu-id="b5f57-260">интеграция .NET</span><span class="sxs-lookup"><span data-stu-id="b5f57-260">.NET integration</span></span>

<span data-ttu-id="b5f57-261">Команда разработчиков EF Core также работает с несколькими сопутствующими независимыми технологиями.</span><span class="sxs-lookup"><span data-stu-id="b5f57-261">The EF Core team also works on several related but independent technologies.</span></span> <span data-ttu-id="b5f57-262">В частности, планируется работа в следующих направлениях.</span><span class="sxs-lookup"><span data-stu-id="b5f57-262">In particular, we plan to work on:</span></span>

### <a name="enhancements-to-systemdata"></a><span data-ttu-id="b5f57-263">Улучшения в System.Data</span><span class="sxs-lookup"><span data-stu-id="b5f57-263">Enhancements to System.Data</span></span>

<span data-ttu-id="b5f57-264">Отслеживается с помощью [проблем в репозитории dotnet\runtime с меткой `area-System.Data` в вехе 6.0](https://github.com/dotnet/runtime/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-System.Data+milestone%3A6.0.0)</span><span class="sxs-lookup"><span data-stu-id="b5f57-264">Tracked by [issues in the dotnet\runtime repo labeled with `area-System.Data` in the 6.0 milestone](https://github.com/dotnet/runtime/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-System.Data+milestone%3A6.0.0)</span></span>

<span data-ttu-id="b5f57-265">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-265">Status: Not started</span></span>

<span data-ttu-id="b5f57-266">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-266">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-267">В этом направлении будут вестись следующие работы.</span><span class="sxs-lookup"><span data-stu-id="b5f57-267">This work includes:</span></span>

- <span data-ttu-id="b5f57-268">Реализация нового [API пакетной обработки](https://github.com/dotnet/runtime/issues/28633).</span><span class="sxs-lookup"><span data-stu-id="b5f57-268">Implementation of the new [batching API](https://github.com/dotnet/runtime/issues/28633).</span></span>
- <span data-ttu-id="b5f57-269">Сотрудничество с другими командами разработчиков .NET и сообществом по вопросам понимания и дальнейшего развития ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="b5f57-269">Continued work with other .NET teams and the community to understand and evolve ADO.NET.</span></span>
- <span data-ttu-id="b5f57-270">[Стандартизация применения DiagnosticSource для трассировки в компонентах System.Data.\*](https://github.com/dotnet/runtime/issues/22336).</span><span class="sxs-lookup"><span data-stu-id="b5f57-270">[Standardize on DiagnosticSource for tracing in System.Data.\* components](https://github.com/dotnet/runtime/issues/22336).</span></span>

### <a name="enhancements-to-microsoftdatasqlite"></a><span data-ttu-id="b5f57-271">Улучшения в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="b5f57-271">Enhancements to Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="b5f57-272">Отслеживается с помощью [проблем с метками `type-enhancement` и `area-adonet-sqlite` в вехе 6.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0+label%3Atype-enhancement+label%3Aarea-adonet-sqlite)</span><span class="sxs-lookup"><span data-stu-id="b5f57-272">Tracked by [issues labeled with `type-enhancement` and `area-adonet-sqlite` in the 6.0 milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A6.0.0+label%3Atype-enhancement+label%3Aarea-adonet-sqlite)</span></span>

<span data-ttu-id="b5f57-273">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="b5f57-273">Status: In-progress</span></span>

<span data-ttu-id="b5f57-274">Размер футболки: Средний</span><span class="sxs-lookup"><span data-stu-id="b5f57-274">T-shirt size: Medium</span></span>

<span data-ttu-id="b5f57-275">Мы планируем внести ряд небольших улучшений для Microsoft.Data.Sqlite, включая [организацию пула соединений](https://github.com/dotnet/efcore/issues/13837) и [использование подготовленных инструкций](https://github.com/dotnet/efcore/issues/14044) для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="b5f57-275">Several small improvements are planned for the Microsoft.Data.Sqlite, including [connection pooling](https://github.com/dotnet/efcore/issues/13837) and [prepared statements](https://github.com/dotnet/efcore/issues/14044) for performance.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="b5f57-276">Ссылочные типы, допускающие значение null</span><span class="sxs-lookup"><span data-stu-id="b5f57-276">Nullable reference types</span></span>

<span data-ttu-id="b5f57-277">Отслеживается по проблеме [№ 14150](https://github.com/dotnet/efcore/issues/14150)</span><span class="sxs-lookup"><span data-stu-id="b5f57-277">Tracked by [#14150](https://github.com/dotnet/efcore/issues/14150)</span></span>

<span data-ttu-id="b5f57-278">Состояние: ведутся работы</span><span class="sxs-lookup"><span data-stu-id="b5f57-278">Status: In-progress</span></span>

<span data-ttu-id="b5f57-279">Размер футболки: большой</span><span class="sxs-lookup"><span data-stu-id="b5f57-279">T-shirt size: Large</span></span>

<span data-ttu-id="b5f57-280">Мы добавим в код EF Core заметки по использованию [ссылочных типов, допускающих значения NULL](/dotnet/csharp/nullable-references).</span><span class="sxs-lookup"><span data-stu-id="b5f57-280">We will annotate the EF Core code to use [nullable reference types](/dotnet/csharp/nullable-references).</span></span>

## <a name="experiments-and-investigations"></a><span data-ttu-id="b5f57-281">Эксперименты и исследования</span><span class="sxs-lookup"><span data-stu-id="b5f57-281">Experiments and investigations</span></span>

<span data-ttu-id="b5f57-282">Команда разработчиков EF в процессе работы над выпуском EF Core 6.0 планирует уделять время экспериментам и исследованиям в двух областях.</span><span class="sxs-lookup"><span data-stu-id="b5f57-282">The EF team is planning to invest time during the EF Core 6.0 timeframe experimenting and investigating in two areas.</span></span> <span data-ttu-id="b5f57-283">Мы рассматриваем это как процесс обучения и поэтому не устанавливаем никаких конкретных конечных показателей в этих областях для выпуска 6.0.</span><span class="sxs-lookup"><span data-stu-id="b5f57-283">This is a learning process and as such no concrete deliverables are planned for the 6.0 release.</span></span>

### <a name="sqlservercore"></a><span data-ttu-id="b5f57-284">SqlServer.Core</span><span class="sxs-lookup"><span data-stu-id="b5f57-284">SqlServer.Core</span></span>

<span data-ttu-id="b5f57-285">Отслеживается в [репозитории лаборатории данных .NET](https://github.com/dotnet/datalab/)</span><span class="sxs-lookup"><span data-stu-id="b5f57-285">Tracked in the [.NET Data Lab repo](https://github.com/dotnet/datalab/)</span></span>

<span data-ttu-id="b5f57-286">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-286">Status: Not started</span></span>

<span data-ttu-id="b5f57-287">Размер футболки: продолжается</span><span class="sxs-lookup"><span data-stu-id="b5f57-287">T-shirt size: Ongoing</span></span>

<span data-ttu-id="b5f57-288">[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) — это полнофункциональный поставщик базы данных ADO.NET для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b5f57-288">[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) is a fully-featured ADO.NET database provider for SQL Server.</span></span> <span data-ttu-id="b5f57-289">Он поддерживает широкий спектр возможностей SQL Server в .NET Core и .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b5f57-289">It supports a broad range of SQL Server features on both .NET Core and .NET Framework.</span></span> <span data-ttu-id="b5f57-290">Однако при этом он содержит большую устаревшую базу кода с множеством сложных взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="b5f57-290">However, it is also a large and old codebase with many complex interactions between its behaviors.</span></span> <span data-ttu-id="b5f57-291">Это затрудняет исследование потенциальных преимуществ, которые можно получить за счет использования новых функций .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5f57-291">This makes it difficult to investigate the potential gains the could be made using newer .NET Core features.</span></span> <span data-ttu-id="b5f57-292">Таким образом, мы начинаем совместный с сообществом эксперимент по определению возможностей для создания высокопроизводительного драйвера SQL Server для .NET.</span><span class="sxs-lookup"><span data-stu-id="b5f57-292">Therefore, we are starting an experiment in collaboration with the community to determine what potential there is for a highly performing SQL Server driver for .NET.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5f57-293">Работа над Microsoft.Data.SqlClient при этом продолжится в прежнем объеме.</span><span class="sxs-lookup"><span data-stu-id="b5f57-293">Investment in Microsoft.Data.SqlClient is not changing.</span></span> <span data-ttu-id="b5f57-294">Это по-прежнему будет рекомендованный способ подключения к SQL Server и SQL Azure как при использовании EF Core, так и в других случаях.</span><span class="sxs-lookup"><span data-stu-id="b5f57-294">It will continue to be the recommended way to connect to SQL Server and SQL Azure, both with and without EF Core.</span></span> <span data-ttu-id="b5f57-295">Также этот поставщик будет поддерживать появляющиеся новые возможности SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b5f57-295">It will continue to support new SQL Server features as they are introduced.</span></span>

### <a name="graphql"></a><span data-ttu-id="b5f57-296">GraphQL</span><span class="sxs-lookup"><span data-stu-id="b5f57-296">GraphQL</span></span>

<span data-ttu-id="b5f57-297">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-297">Status: Not started</span></span>

<span data-ttu-id="b5f57-298">Размер футболки: продолжается</span><span class="sxs-lookup"><span data-stu-id="b5f57-298">T-shirt size: Ongoing</span></span>

<span data-ttu-id="b5f57-299">В последние несколько лет [GraphQL](https://graphql.org/) все активнее используется на различных платформах.</span><span class="sxs-lookup"><span data-stu-id="b5f57-299">[GraphQL](https://graphql.org/) has been gaining traction over the last few years across a variety of platforms.</span></span> <span data-ttu-id="b5f57-300">Мы планируем исследовать это направление в поисках способов улучшить работу с .NET.</span><span class="sxs-lookup"><span data-stu-id="b5f57-300">We plan to investigate the space and find ways to improve the experience with .NET.</span></span> <span data-ttu-id="b5f57-301">Эта задача предполагает взаимодействие с сообществом в вопросах понимания и поддержки существующей экосистемы.</span><span class="sxs-lookup"><span data-stu-id="b5f57-301">This will involve working with the community on understanding and supporting the existing ecosystem.</span></span> <span data-ttu-id="b5f57-302">Корпорация Майкрософт может участвовать в этом процессе посредством вклада в существующие наработки или разработки бесплатных компонентов для стека Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="b5f57-302">It may also involve specific investment from Microsoft, either in the form of contributions to existing work or in developing complimentary pieces in the Microsoft stack.</span></span>

### <a name="dataverse-formerly-common-data-services"></a><span data-ttu-id="b5f57-303">DataVerse (ранее — Common Data Services)</span><span class="sxs-lookup"><span data-stu-id="b5f57-303">DataVerse (formerly Common Data Services)</span></span>

<span data-ttu-id="b5f57-304">Состояние: работа не начата</span><span class="sxs-lookup"><span data-stu-id="b5f57-304">Status: Not started</span></span>

<span data-ttu-id="b5f57-305">Размер футболки: продолжается</span><span class="sxs-lookup"><span data-stu-id="b5f57-305">T-shirt size: Ongoing</span></span>

<span data-ttu-id="b5f57-306">[DataVerse](/powerapps/maker/data-platform/data-platform-intro) — это хранилище данных на основе столбцов, предназначенное для быстрой разработки бизнес-приложений.</span><span class="sxs-lookup"><span data-stu-id="b5f57-306">[DataVerse](/powerapps/maker/data-platform/data-platform-intro) is a column-based data store designed for rapid development of business applications.</span></span> <span data-ttu-id="b5f57-307">Оно обеспечивает автоматическую обработку сложных типов данных, например двоичных объектов (BLOB), и имеет встроенные сущности и связи, такие как организации и контакты.</span><span class="sxs-lookup"><span data-stu-id="b5f57-307">It automatically handles complex data types like binary objects (BLOBs) and has built-in entities and relationships like organizations and contacts.</span></span> <span data-ttu-id="b5f57-308">Для этого хранилища предусмотрен пакет SDK, однако разработчики могут воспользоваться преимуществами поставщика EF Core, позволяющими использовать расширенные запросы LINQ, единицы работы и единый API доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="b5f57-308">An SDK exists but developers may benefit from having an EF Core provider to use advanced LINQ queries, take advantage of unit of work and have a consistent data access API.</span></span> <span data-ttu-id="b5f57-309">Наша команда будет рассматривать различные способы улучшения взаимодействия с разработчиками .NET, использующих DataVerse.</span><span class="sxs-lookup"><span data-stu-id="b5f57-309">The team will consider different ways to improve the .NET developer experience connecting to DataVerse.</span></span>

## <a name="below-the-cut-line"></a><span data-ttu-id="b5f57-310">Сопутствующие вопросы</span><span class="sxs-lookup"><span data-stu-id="b5f57-310">Below-the-cut-line</span></span>

<span data-ttu-id="b5f57-311">Отслеживание с помощью [вопроса, отмеченного `consider-for-current-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-current-release)</span><span class="sxs-lookup"><span data-stu-id="b5f57-311">Tracked by [issues labeled with `consider-for-current-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-current-release)</span></span>

<span data-ttu-id="b5f57-312">Это исправления ошибок и улучшения, которые пока **не** запланированы для выпуска 6.0, но мы будем рассматривать их в зависимости от того, как будет продвигаться описанная выше работа.</span><span class="sxs-lookup"><span data-stu-id="b5f57-312">These are bug fixes and enhancements that are **not** currently scheduled for the 6.0 release, but we will look at based on feedback throughout the release together with progress made on the work above.</span></span> <span data-ttu-id="b5f57-313">Эти проблемы могут быть включены в процесс работы над выпуском EF Core 6.0 и автоматически становятся кандидатами на решение в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="b5f57-313">These issues may be pulled in to EF Core 6.0, and automatically become candidates for the next release.</span></span>

<span data-ttu-id="b5f57-314">Кроме того, при планировании мы всегда учитываем [вопросы, набравшие больше всего голосов](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="b5f57-314">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="b5f57-315">Исключение любой из подобных проблем из выпуска всегда дается нелегко, но нам нужен реалистичный план с учетом ресурсов, которыми мы располагаем.</span><span class="sxs-lookup"><span data-stu-id="b5f57-315">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span> <span data-ttu-id="b5f57-316">Обязательно проголосуйте (👍) за интересующие вас возможности.</span><span class="sxs-lookup"><span data-stu-id="b5f57-316">Make sure you have voted (👍) for the features you need.</span></span>

## <a name="suggestions"></a><span data-ttu-id="b5f57-317">Предложения</span><span class="sxs-lookup"><span data-stu-id="b5f57-317">Suggestions</span></span>

<span data-ttu-id="b5f57-318">Нам важно ваше мнение о планировании.</span><span class="sxs-lookup"><span data-stu-id="b5f57-318">Your feedback on planning is important.</span></span> <span data-ttu-id="b5f57-319">Лучший способ указать важность проблемы — проголосовать за нее на GitHub (👍).</span><span class="sxs-lookup"><span data-stu-id="b5f57-319">The best way to indicate the importance of an issue is to vote (👍) for that issue on GitHub.</span></span> <span data-ttu-id="b5f57-320">Эти данные будут учитываться в [процессе планирования](xref:core/what-is-new/release-planning) для следующего выпуска.</span><span class="sxs-lookup"><span data-stu-id="b5f57-320">This data will then feed into the [planning process](xref:core/what-is-new/release-planning) for the next release.</span></span>
