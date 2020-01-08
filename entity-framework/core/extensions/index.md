---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502086"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="c761f-102">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="c761f-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="c761f-103">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="c761f-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c761f-104">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c761f-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c761f-105">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="c761f-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="c761f-106">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="c761f-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="c761f-107">Инструменты</span><span class="sxs-lookup"><span data-stu-id="c761f-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="c761f-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="c761f-108">LLBLGen Pro</span></span>

<span data-ttu-id="c761f-109">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c761f-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="c761f-110">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="c761f-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="c761f-111">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-111">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-112">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="c761f-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="c761f-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="c761f-113">Devart Entity Developer</span></span>

<span data-ttu-id="c761f-114">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="c761f-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="c761f-115">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="c761f-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="c761f-116">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-116">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-117">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="c761f-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="c761f-118">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="c761f-118">EF Core Power Tools</span></span>

<span data-ttu-id="c761f-119">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="c761f-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="c761f-120">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="c761f-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="c761f-121">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c761f-122">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="c761f-123">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c761f-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="c761f-124">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="c761f-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="c761f-125">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="c761f-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="c761f-126">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="c761f-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="c761f-127">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-127">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="c761f-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="c761f-129">CatFactory</span><span class="sxs-lookup"><span data-stu-id="c761f-129">CatFactory</span></span>

<span data-ttu-id="c761f-130">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c761f-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="c761f-131">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-131">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-132">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="c761f-133">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="c761f-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="c761f-134">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="c761f-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="c761f-135">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="c761f-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="c761f-136">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-136">For EF Core: 2.</span></span>

<span data-ttu-id="c761f-137">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="c761f-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="c761f-138">Расширения</span><span class="sxs-lookup"><span data-stu-id="c761f-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="c761f-139">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="c761f-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="c761f-140">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="c761f-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="c761f-141">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-141">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-142">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="c761f-143">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="c761f-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="c761f-144">Расширение, которое позволяет хранить результаты запросов EF Core в кэше второго уровня, чтобы при последующем выполнении тех же запросов не обращаться к базе данных и получить данные напрямую из кэша.</span><span class="sxs-lookup"><span data-stu-id="c761f-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="c761f-145">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-145">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-146">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="c761f-147">Geco</span><span class="sxs-lookup"><span data-stu-id="c761f-147">Geco</span></span>

<span data-ttu-id="c761f-148">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="c761f-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="c761f-149">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c761f-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="c761f-150">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="c761f-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="c761f-151">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-151">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-152">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="c761f-153">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="c761f-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="c761f-154">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="c761f-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="c761f-155">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c761f-156">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="c761f-157">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c761f-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="c761f-158">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="c761f-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="c761f-159">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c761f-160">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="c761f-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="c761f-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="c761f-162">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="c761f-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="c761f-163">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-163">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-164">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="c761f-165">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="c761f-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="c761f-166">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="c761f-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="c761f-167">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c761f-168">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="c761f-169">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="c761f-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="c761f-170">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="c761f-170">Adds design-time pluralization.</span></span> <span data-ttu-id="c761f-171">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-171">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-172">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="c761f-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="c761f-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="c761f-174">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="c761f-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="c761f-175">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c761f-176">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="c761f-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="c761f-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="c761f-178">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="c761f-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="c761f-179">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="c761f-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="c761f-180">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-180">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-181">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="c761f-182">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="c761f-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="c761f-183">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="c761f-183">An implementation of temporal support.</span></span> <span data-ttu-id="c761f-184">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-184">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-185">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="c761f-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="c761f-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="c761f-187">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="c761f-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="c761f-188">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-188">For EF Core: 3.</span></span>

[<span data-ttu-id="c761f-189">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="c761f-190">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="c761f-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="c761f-191">Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.</span><span class="sxs-lookup"><span data-stu-id="c761f-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="c761f-192">Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="c761f-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="c761f-193">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-193">For EF Core: 3.</span></span>

[<span data-ttu-id="c761f-194">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="c761f-195">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="c761f-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="c761f-196">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="c761f-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="c761f-197">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-197">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-198">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="c761f-199">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="c761f-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="c761f-200">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="c761f-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="c761f-201">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c761f-201">For EF Core: 2.</span></span>

[<span data-ttu-id="c761f-202">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="c761f-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="c761f-203">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="c761f-203">Entity Framework Plus</span></span>

<span data-ttu-id="c761f-204">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="c761f-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="c761f-205">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="c761f-206">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="c761f-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="c761f-207">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c761f-207">Entity Framework Extensions</span></span>

<span data-ttu-id="c761f-208">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="c761f-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="c761f-209">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c761f-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c761f-210">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="c761f-210">Website</span></span>](https://entityframework-extensions.net/)
