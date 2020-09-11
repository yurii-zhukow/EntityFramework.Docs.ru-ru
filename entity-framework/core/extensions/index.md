---
title: Инструменты и расширения — EF Core
description: Внешние средства и расширения для Entity Framework Core
author: ErikEJ
ms.date: 04/11/2020
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e8198e0b0a51968336be97ea7d7a4b65e856d32f
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618021"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="e7894-103">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="e7894-103">EF Core Tools & Extensions</span></span>

<span data-ttu-id="e7894-104">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="e7894-104">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="e7894-105">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-105">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="e7894-106">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="e7894-106">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="e7894-107">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="e7894-107">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="e7894-108">Инструменты</span><span class="sxs-lookup"><span data-stu-id="e7894-108">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="e7894-109">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="e7894-109">LLBLGen Pro</span></span>

<span data-ttu-id="e7894-110">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-110">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="e7894-111">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="e7894-111">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="e7894-112">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-112">For EF Core: 2, 3</span></span>

[<span data-ttu-id="e7894-113">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="e7894-113">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="e7894-114">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="e7894-114">Devart Entity Developer</span></span>

<span data-ttu-id="e7894-115">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="e7894-115">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="e7894-116">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="e7894-116">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="e7894-117">Для EF Core: 1, 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="e7894-117">For EF Core: 1, 2, 3, 5.</span></span>

[<span data-ttu-id="e7894-118">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="e7894-118">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="e7894-119">Использование nHydrate ORM для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e7894-119">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="e7894-120">ORM создает строго типизированные расширяемые классы для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e7894-120">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="e7894-121">Созданный код представляет собой Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-121">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="e7894-122">Различий при этом нет.</span><span class="sxs-lookup"><span data-stu-id="e7894-122">There is no difference.</span></span> <span data-ttu-id="e7894-123">Это не замена EF или пользовательского ORM.</span><span class="sxs-lookup"><span data-stu-id="e7894-123">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="e7894-124">Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных.</span><span class="sxs-lookup"><span data-stu-id="e7894-124">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="e7894-125">Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами.</span><span class="sxs-lookup"><span data-stu-id="e7894-125">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="e7894-126">Установщик отслеживает изменения модели и создает скрипты обновления.</span><span class="sxs-lookup"><span data-stu-id="e7894-126">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="e7894-127">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-127">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-128">Сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-128">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="e7894-129">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="e7894-129">EF Core Power Tools</span></span>

<span data-ttu-id="e7894-130">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="e7894-130">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="e7894-131">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="e7894-131">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="e7894-132">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-132">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-133">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-133">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="e7894-134">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e7894-134">Entity Framework Visual Editor</span></span>

<span data-ttu-id="e7894-135">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-135">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="e7894-136">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="e7894-136">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="e7894-137">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="e7894-137">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="e7894-138">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-138">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-139">Marketplace</span><span class="sxs-lookup"><span data-stu-id="e7894-139">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="e7894-140">CatFactory</span><span class="sxs-lookup"><span data-stu-id="e7894-140">CatFactory</span></span>

<span data-ttu-id="e7894-141">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e7894-141">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="e7894-142">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-142">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-143">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-143">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="e7894-144">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="e7894-144">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="e7894-145">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="e7894-145">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="e7894-146">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="e7894-146">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="e7894-147">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-147">For EF Core: 2.</span></span>

<span data-ttu-id="e7894-148">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="e7894-148">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="e7894-149">Расширения</span><span class="sxs-lookup"><span data-stu-id="e7894-149">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="e7894-150">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="e7894-150">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="e7894-151">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="e7894-151">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="e7894-152">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-152">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-153">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-153">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efcoresecondlevelcacheinterceptor"></a><span data-ttu-id="e7894-154">EFCoreSecondLevelCacheInterceptor</span><span class="sxs-lookup"><span data-stu-id="e7894-154">EFCoreSecondLevelCacheInterceptor</span></span>

<span data-ttu-id="e7894-155">Кэширование второго уровня — это кэширование запросов.</span><span class="sxs-lookup"><span data-stu-id="e7894-155">Second level caching is a query cache.</span></span> <span data-ttu-id="e7894-156">Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.</span><span class="sxs-lookup"><span data-stu-id="e7894-156">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span> <span data-ttu-id="e7894-157">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-157">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-158">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-158">GitHub repository</span></span>](https://github.com/VahidN/EFCoreSecondLevelCacheInterceptor)

### <a name="geco"></a><span data-ttu-id="e7894-159">Geco</span><span class="sxs-lookup"><span data-stu-id="e7894-159">Geco</span></span>

<span data-ttu-id="e7894-160">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="e7894-160">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="e7894-161">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="e7894-161">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="e7894-162">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="e7894-162">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="e7894-163">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-163">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-164">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-164">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="e7894-165">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="e7894-165">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="e7894-166">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="e7894-166">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="e7894-167">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-168">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-168">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="e7894-169">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="e7894-169">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="e7894-170">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="e7894-170">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="e7894-171">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-171">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-172">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-172">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="e7894-173">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="e7894-173">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="e7894-174">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="e7894-174">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="e7894-175">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-175">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-176">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-176">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="e7894-177">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="e7894-177">EFCore.BulkExtensions</span></span>

<span data-ttu-id="e7894-178">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="e7894-178">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="e7894-179">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-179">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-180">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-180">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="e7894-181">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="e7894-181">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="e7894-182">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="e7894-182">Adds design-time pluralization.</span></span> <span data-ttu-id="e7894-183">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-183">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-184">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-184">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="e7894-185">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="e7894-185">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="e7894-186">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="e7894-186">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="e7894-187">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-187">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-188">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-188">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="e7894-189">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="e7894-189">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="e7894-190">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-190">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="e7894-191">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="e7894-191">Makes it act more like a relational provider.</span></span> <span data-ttu-id="e7894-192">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-192">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-193">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-193">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="e7894-194">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="e7894-194">EFCore.TemporalSupport</span></span>

<span data-ttu-id="e7894-195">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="e7894-195">An implementation of temporal support.</span></span> <span data-ttu-id="e7894-196">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-196">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-197">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-197">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="e7894-198">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="e7894-198">EfCoreTemporalTable</span></span>

<span data-ttu-id="e7894-199">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="e7894-199">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="e7894-200">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-200">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-201">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-201">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="e7894-202">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="e7894-202">EFCore.TimeTraveler</span></span>

<span data-ttu-id="e7894-203">Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-203">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="e7894-204">Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="e7894-204">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="e7894-205">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-205">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-206">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-206">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="e7894-207">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="e7894-207">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="e7894-208">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="e7894-208">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="e7894-209">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-209">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-210">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-210">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="e7894-211">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="e7894-211">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="e7894-212">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="e7894-212">A high-performance second-level query cache.</span></span> <span data-ttu-id="e7894-213">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-213">For EF Core: 2.</span></span>

[<span data-ttu-id="e7894-214">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-214">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entityframeworkcorencache"></a><span data-ttu-id="e7894-215">EntityFrameworkCore.NCache</span><span class="sxs-lookup"><span data-stu-id="e7894-215">EntityFrameworkCore.NCache</span></span>

<span data-ttu-id="e7894-216">Поставщик Entity Framework Core NCache является распределенным поставщиком кэша второго уровня для кэширования результатов запросов.</span><span class="sxs-lookup"><span data-stu-id="e7894-216">NCache Entity Framework Core Provider is a distributed second level cache provider for caching query results.</span></span> <span data-ttu-id="e7894-217">Распределенная архитектура NCache повышает уровень его масштабируемости и высокой доступности.</span><span class="sxs-lookup"><span data-stu-id="e7894-217">The distributed architecture of NCache makes it more scalable and highly available.</span></span> <span data-ttu-id="e7894-218">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="e7894-218">For EF Core 2.</span></span>

[<span data-ttu-id="e7894-219">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="e7894-219">Website</span></span>](https://www.alachisoft.com/ncache/ef-core-cache.html)

### <a name="entity-framework-plus"></a><span data-ttu-id="e7894-220">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="e7894-220">Entity Framework Plus</span></span>

<span data-ttu-id="e7894-221">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="e7894-221">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="e7894-222">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-222">For EF Core: 2, 3.</span></span>

<span data-ttu-id="e7894-223">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="e7894-223">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="e7894-224">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e7894-224">Entity Framework Extensions</span></span>

<span data-ttu-id="e7894-225">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="e7894-225">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="e7894-226">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-226">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-227">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="e7894-227">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="e7894-228">Expressionify</span><span class="sxs-lookup"><span data-stu-id="e7894-228">Expressionify</span></span>

<span data-ttu-id="e7894-229">Добавляет поддержку вызова методов расширения в лямбда-выражениях LINQ.</span><span class="sxs-lookup"><span data-stu-id="e7894-229">Add support for calling extension methods in LINQ lambdas.</span></span> <span data-ttu-id="e7894-230">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-230">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-231">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-231">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="e7894-232">XLinq</span><span class="sxs-lookup"><span data-stu-id="e7894-232">XLinq</span></span>

<span data-ttu-id="e7894-233">Технология языка интегрированных запросов (LINQ) для реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="e7894-233">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="e7894-234">Позволяет писать строго типизированные запросы на C#.</span><span class="sxs-lookup"><span data-stu-id="e7894-234">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="e7894-235">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-235">For EF Core: 3.</span></span>

- <span data-ttu-id="e7894-236">Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.</span><span class="sxs-lookup"><span data-stu-id="e7894-236">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="e7894-237">Отсутствие семантического разрыва с SQL.</span><span class="sxs-lookup"><span data-stu-id="e7894-237">No semantic gap with SQL.</span></span> <span data-ttu-id="e7894-238">XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.</span><span class="sxs-lookup"><span data-stu-id="e7894-238">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="e7894-239">В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .</span><span class="sxs-lookup"><span data-stu-id="e7894-239">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="e7894-240">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="e7894-240">Website</span></span>](http://xlinq.live/)

### <a name="ramses"></a><span data-ttu-id="e7894-241">Ramses</span><span class="sxs-lookup"><span data-stu-id="e7894-241">Ramses</span></span>

<span data-ttu-id="e7894-242">Перехватчики жизненного цикла (для SaveChanges).</span><span class="sxs-lookup"><span data-stu-id="e7894-242">Lifecycle hooks (for SaveChanges).</span></span> <span data-ttu-id="e7894-243">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-243">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="e7894-244">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-244">GitHub repository</span></span>](https://github.com/JValck/Ramses)

### <a name="efcorenamingconventions"></a><span data-ttu-id="e7894-245">EFCore.NamingConventions</span><span class="sxs-lookup"><span data-stu-id="e7894-245">EFCore.NamingConventions</span></span>

<span data-ttu-id="e7894-246">Автоматически преобразует имена всех таблиц и столбцов для использования "змеиного_регистра", ВСЕХ ПРОПИСНЫХ или всех строчных букв.</span><span class="sxs-lookup"><span data-stu-id="e7894-246">This will automatically make all your table and column names have snake_case, all UPPER or all lower case naming.</span></span> <span data-ttu-id="e7894-247">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-247">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-248">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-248">GitHub repository</span></span>](https://github.com/efcore/EFCore.NamingConventions)

### <a name="simplersoftwareentityframeworkcoresqlservernodatime"></a><span data-ttu-id="e7894-249">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span><span class="sxs-lookup"><span data-stu-id="e7894-249">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span></span>

<span data-ttu-id="e7894-250">Добавляет встроенную поддержку типов NodaTime в EntityFrameworkCore для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e7894-250">Adds native support to EntityFrameworkCore for SQL Server for the NodaTime types.</span></span> <span data-ttu-id="e7894-251">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-251">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-252">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-252">GitHub repository</span></span>](https://github.com/StevenRasmussen/EFCore.SqlServer.NodaTime)

### <a name="dabbleentityframeworkcoretemporalquery"></a><span data-ttu-id="e7894-253">Dabble.EntityFrameworkCore.Temporal.Query</span><span class="sxs-lookup"><span data-stu-id="e7894-253">Dabble.EntityFrameworkCore.Temporal.Query</span></span>

<span data-ttu-id="e7894-254">LINQ-расширения Entity Framework Core 3.1 для поддержки запросов во временных таблицах Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e7894-254">LINQ extensions to Entity Framework Core 3.1 to support Microsoft SQL Server Temporal Table Querying.</span></span> <span data-ttu-id="e7894-255">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-255">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-256">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-256">GitHub repository</span></span>](https://github.com/Adam-Langley/efcore-temporal-query)

### <a name="entityframeworkcoresqlserverhierarchyid"></a><span data-ttu-id="e7894-257">EntityFrameworkCore.SqlServer.HierarchyId</span><span class="sxs-lookup"><span data-stu-id="e7894-257">EntityFrameworkCore.SqlServer.HierarchyId</span></span>

<span data-ttu-id="e7894-258">Добавляет поддержку hierarchyid в поставщик SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7894-258">Adds hierarchyid support to the SQL Server EF Core provider.</span></span> <span data-ttu-id="e7894-259">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-259">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-260">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-260">GitHub repository</span></span>](https://github.com/efcore/EFCore.SqlServer.HierarchyId)

### <a name="linq2dbentityframeworkcore"></a><span data-ttu-id="e7894-261">linq2db.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="e7894-261">linq2db.EntityFrameworkCore</span></span>

<span data-ttu-id="e7894-262">Альтернативный переводчик запросов LINQ к выражениям SQL.</span><span class="sxs-lookup"><span data-stu-id="e7894-262">Alternative translator of LINQ queries to SQL expressions.</span></span> <span data-ttu-id="e7894-263">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-263">For EF Core: 3.</span></span>

<span data-ttu-id="e7894-264">Включает поддержку расширенных функций SQL, таких как выражения CTE, массовое копирование, табличные указания, оконные функции, временные таблицы и операции создания, обновления и удаления на стороне базы данных.</span><span class="sxs-lookup"><span data-stu-id="e7894-264">Includes support for advanced SQL features such as CTEs, bulk copy, table hints, windowed functions, temporary tables, and database-side create/update/delete operations.</span></span>

[<span data-ttu-id="e7894-265">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="e7894-265">GitHub repository</span></span>](https://github.com/linq2db/linq2db.EntityFrameworkCore)

### <a name="efcoresoftdelete"></a><span data-ttu-id="e7894-266">EFCore.SoftDelete</span><span class="sxs-lookup"><span data-stu-id="e7894-266">EFCore.SoftDelete</span></span>

<span data-ttu-id="e7894-267">Реализация для обратимого удаления сущностей.</span><span class="sxs-lookup"><span data-stu-id="e7894-267">An implementation for soft deleting entities.</span></span> <span data-ttu-id="e7894-268">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="e7894-268">For EF Core: 3.</span></span>

[<span data-ttu-id="e7894-269">NuGet</span><span class="sxs-lookup"><span data-stu-id="e7894-269">NuGet</span></span>](https://www.nuget.org/packages/EFCore.SoftDelete)
