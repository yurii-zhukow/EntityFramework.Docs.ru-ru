---
title: Инструменты и расширения — EF Core
description: Внешние средства и расширения для Entity Framework Core
author: ErikEJ
ms.date: 02/21/2021
uid: core/extensions/index
ms.openlocfilehash: 67d078250d8bfdb845cf98949ee1412a8fe842bd
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024385"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="463f6-103">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="463f6-103">EF Core Tools & Extensions</span></span>

<span data-ttu-id="463f6-104">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="463f6-104">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="463f6-105">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="463f6-105">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="463f6-106">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="463f6-106">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="463f6-107">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="463f6-107">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="463f6-108">Инструменты</span><span class="sxs-lookup"><span data-stu-id="463f6-108">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="463f6-109">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="463f6-109">LLBLGen Pro</span></span>

<span data-ttu-id="463f6-110">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="463f6-110">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="463f6-111">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="463f6-111">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="463f6-112">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-112">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-113">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="463f6-113">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="463f6-114">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="463f6-114">Devart Entity Developer</span></span>

<span data-ttu-id="463f6-115">Entity Developer — мощный конструктор O/RM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="463f6-115">Entity Developer is a powerful O/RM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="463f6-116">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="463f6-116">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="463f6-117">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-117">For EF Core: 2, 3, 5.</span></span>

[<span data-ttu-id="463f6-118">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="463f6-118">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="463f6-119">Использование nHydrate ORM для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="463f6-119">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="463f6-120">O/RM создает строго типизированные расширяемые классы для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="463f6-120">An O/RM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="463f6-121">Созданный код представляет собой Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="463f6-121">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="463f6-122">Различий при этом нет.</span><span class="sxs-lookup"><span data-stu-id="463f6-122">There is no difference.</span></span> <span data-ttu-id="463f6-123">Это не замена EF или пользовательского O/RM.</span><span class="sxs-lookup"><span data-stu-id="463f6-123">This is not a replacement for EF or a custom O/RM.</span></span> <span data-ttu-id="463f6-124">Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных.</span><span class="sxs-lookup"><span data-stu-id="463f6-124">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="463f6-125">Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами.</span><span class="sxs-lookup"><span data-stu-id="463f6-125">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="463f6-126">Установщик отслеживает изменения модели и создает скрипты обновления.</span><span class="sxs-lookup"><span data-stu-id="463f6-126">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="463f6-127">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-127">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-128">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-128">Github repository</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="463f6-129">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="463f6-129">EF Core Power Tools</span></span>

<span data-ttu-id="463f6-130">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="463f6-130">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="463f6-131">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="463f6-131">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="463f6-132">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-132">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-133">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-133">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="463f6-134">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="463f6-134">Entity Framework Visual Editor</span></span>

<span data-ttu-id="463f6-135">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор O/RM для визуального проектирования классов EF 6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="463f6-135">Entity Framework Visual Editor is a Visual Studio extension that adds an O/RM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="463f6-136">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="463f6-136">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="463f6-137">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="463f6-137">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="463f6-138">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-138">For EF Core: 2, 3, 5.</span></span>

[<span data-ttu-id="463f6-139">Marketplace</span><span class="sxs-lookup"><span data-stu-id="463f6-139">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="463f6-140">CatFactory</span><span class="sxs-lookup"><span data-stu-id="463f6-140">CatFactory</span></span>

<span data-ttu-id="463f6-141">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="463f6-141">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="463f6-142">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="463f6-142">For EF Core: 2.</span></span>

[<span data-ttu-id="463f6-143">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-143">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="463f6-144">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="463f6-144">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="463f6-145">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="463f6-145">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="463f6-146">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="463f6-146">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="463f6-147">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="463f6-147">For EF Core: 2.</span></span>

<span data-ttu-id="463f6-148">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="463f6-148">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="463f6-149">Расширения</span><span class="sxs-lookup"><span data-stu-id="463f6-149">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="463f6-150">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="463f6-150">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="463f6-151">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="463f6-151">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="463f6-152">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-152">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-153">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-153">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efcoresecondlevelcacheinterceptor"></a><span data-ttu-id="463f6-154">EFCoreSecondLevelCacheInterceptor</span><span class="sxs-lookup"><span data-stu-id="463f6-154">EFCoreSecondLevelCacheInterceptor</span></span>

<span data-ttu-id="463f6-155">Кэширование второго уровня — это кэширование запросов.</span><span class="sxs-lookup"><span data-stu-id="463f6-155">Second level caching is a query cache.</span></span> <span data-ttu-id="463f6-156">Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.</span><span class="sxs-lookup"><span data-stu-id="463f6-156">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span> <span data-ttu-id="463f6-157">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-157">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-158">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-158">GitHub repository</span></span>](https://github.com/VahidN/EFCoreSecondLevelCacheInterceptor)

### <a name="geco"></a><span data-ttu-id="463f6-159">Geco</span><span class="sxs-lookup"><span data-stu-id="463f6-159">Geco</span></span>

<span data-ttu-id="463f6-160">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="463f6-160">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="463f6-161">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="463f6-161">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="463f6-162">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="463f6-162">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="463f6-163">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="463f6-163">For EF Core: 2.</span></span>

[<span data-ttu-id="463f6-164">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-164">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="463f6-165">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="463f6-165">EntityFrameworkCore.Scaffolding.Handlebars</span></span>

<span data-ttu-id="463f6-166">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="463f6-166">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="463f6-167">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-167">For EF Core: 2, 3, 5.</span></span>

[<span data-ttu-id="463f6-168">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-168">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="463f6-169">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="463f6-169">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="463f6-170">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="463f6-170">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="463f6-171">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-171">For EF Core: 2, 3, 5.</span></span>

[<span data-ttu-id="463f6-172">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-172">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="463f6-173">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="463f6-173">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="463f6-174">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="463f6-174">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="463f6-175">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-176">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-176">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="463f6-177">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="463f6-177">EFCore.BulkExtensions</span></span>

<span data-ttu-id="463f6-178">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="463f6-178">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="463f6-179">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-179">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-180">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-180">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="463f6-181">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="463f6-181">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="463f6-182">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="463f6-182">Adds design-time pluralization.</span></span> <span data-ttu-id="463f6-183">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-183">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-184">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-184">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="463f6-185">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="463f6-185">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="463f6-186">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="463f6-186">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="463f6-187">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-187">For EF Core: 2, 3, 5.</span></span>

[<span data-ttu-id="463f6-188">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-188">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="verifyentityframework"></a><span data-ttu-id="463f6-189">Verify.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="463f6-189">Verify.EntityFramework</span></span>

<span data-ttu-id="463f6-190">Расширяет возможности [Verify](https://github.com/VerifyTests/Verify), чтобы разрешить тестирование моментальных снимков с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="463f6-190">Extends [Verify](https://github.com/VerifyTests/Verify) to allow snapshot testing with EntityFramework.</span></span> <span data-ttu-id="463f6-191">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-191">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-192">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-192">GitHub repository</span></span>](https://github.com/VerifyTests/Verify.EntityFramework)

### <a name="localdb"></a><span data-ttu-id="463f6-193">LocalDB</span><span class="sxs-lookup"><span data-stu-id="463f6-193">LocalDb</span></span>

<span data-ttu-id="463f6-194">Предоставляет оболочку для [SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb), позволяющую упростить выполнение тестов в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="463f6-194">Provides a wrapper around [SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb) to simplify running tests against Entity Framework.</span></span> <span data-ttu-id="463f6-195">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-195">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-196">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-196">GitHub repository</span></span>](https://github.com/SimonCropp/LocalDb)

### <a name="effluentvalidation"></a><span data-ttu-id="463f6-197">EfFluentValidation</span><span class="sxs-lookup"><span data-stu-id="463f6-197">EfFluentValidation</span></span>

<span data-ttu-id="463f6-198">Добавляет поддержку [FluentValidation](https://fluentvalidation.net/) в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="463f6-198">Adds [FluentValidation](https://fluentvalidation.net/) support to Entity Framework.</span></span> <span data-ttu-id="463f6-199">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-199">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-200">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-200">GitHub repository</span></span>](https://github.com/SimonCropp/EfFluentValidation)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="463f6-201">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="463f6-201">EFCore.TemporalSupport</span></span>

<span data-ttu-id="463f6-202">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="463f6-202">An implementation of temporal support.</span></span> <span data-ttu-id="463f6-203">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="463f6-203">For EF Core: 2.</span></span>

[<span data-ttu-id="463f6-204">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-204">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="463f6-205">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="463f6-205">EfCoreTemporalTable</span></span>

<span data-ttu-id="463f6-206">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="463f6-206">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="463f6-207">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-207">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-208">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-208">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="463f6-209">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="463f6-209">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="463f6-210">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="463f6-210">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="463f6-211">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-211">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-212">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-212">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="463f6-213">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="463f6-213">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="463f6-214">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="463f6-214">A high-performance second-level query cache.</span></span> <span data-ttu-id="463f6-215">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="463f6-215">For EF Core: 2.</span></span>

[<span data-ttu-id="463f6-216">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-216">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entityframeworkcorencache"></a><span data-ttu-id="463f6-217">EntityFrameworkCore.NCache</span><span class="sxs-lookup"><span data-stu-id="463f6-217">EntityFrameworkCore.NCache</span></span>

<span data-ttu-id="463f6-218">Поставщик Entity Framework Core NCache является распределенным поставщиком кэша второго уровня для кэширования результатов запросов.</span><span class="sxs-lookup"><span data-stu-id="463f6-218">NCache Entity Framework Core Provider is a distributed second level cache provider for caching query results.</span></span> <span data-ttu-id="463f6-219">Распределенная архитектура NCache повышает уровень его масштабируемости и высокой доступности.</span><span class="sxs-lookup"><span data-stu-id="463f6-219">The distributed architecture of NCache makes it more scalable and highly available.</span></span> <span data-ttu-id="463f6-220">Для EF Core 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-220">For EF Core 2, 3.</span></span>

[<span data-ttu-id="463f6-221">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="463f6-221">Website</span></span>](https://www.alachisoft.com/ncache/ef-core-cache.html)

### <a name="entityframeworkcoretriggered"></a><span data-ttu-id="463f6-222">EntityFrameworkCore.Triggered</span><span class="sxs-lookup"><span data-stu-id="463f6-222">EntityFrameworkCore.Triggered</span></span>

<span data-ttu-id="463f6-223">Триггеры для EF Core.</span><span class="sxs-lookup"><span data-stu-id="463f6-223">Triggers for EF Core.</span></span> <span data-ttu-id="463f6-224">Реагирование на изменения в DbContext до и после фиксации этих изменений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="463f6-224">Respond to changes in your DbContext before and after they are committed to the database.</span></span> <span data-ttu-id="463f6-225">Триггеры полностью асинхронны и поддерживают внедрение зависимостей, наследование, каскадирование и другие возможности.</span><span class="sxs-lookup"><span data-stu-id="463f6-225">Triggers are fully asynchronous and support dependency injection, inheritance, cascading and more.</span></span> <span data-ttu-id="463f6-226">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-226">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-227">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-227">GitHub repository</span></span>](https://github.com/koenbeuk/EntityFrameworkCore.Triggered)

### <a name="entity-framework-plus"></a><span data-ttu-id="463f6-228">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="463f6-228">Entity Framework Plus</span></span>

<span data-ttu-id="463f6-229">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="463f6-229">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="463f6-230">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-230">For EF Core: 2, 3, 5.</span></span>

<span data-ttu-id="463f6-231">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="463f6-231">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="463f6-232">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="463f6-232">Entity Framework Extensions</span></span>

<span data-ttu-id="463f6-233">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="463f6-233">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="463f6-234">Для EF Core: 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-234">For EF Core: 2, 3, 5.</span></span>

[<span data-ttu-id="463f6-235">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="463f6-235">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="463f6-236">Expressionify</span><span class="sxs-lookup"><span data-stu-id="463f6-236">Expressionify</span></span>

<span data-ttu-id="463f6-237">Добавляет поддержку вызова методов расширения в лямбда-выражениях LINQ.</span><span class="sxs-lookup"><span data-stu-id="463f6-237">Add support for calling extension methods in LINQ lambdas.</span></span> <span data-ttu-id="463f6-238">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-238">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-239">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-239">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="elinq"></a><span data-ttu-id="463f6-240">ELinq</span><span class="sxs-lookup"><span data-stu-id="463f6-240">ELinq</span></span>

<span data-ttu-id="463f6-241">Технология языка интегрированных запросов (LINQ) для реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="463f6-241">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="463f6-242">Позволяет писать строго типизированные запросы на C#.</span><span class="sxs-lookup"><span data-stu-id="463f6-242">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="463f6-243">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-243">For EF Core: 3.</span></span>

- <span data-ttu-id="463f6-244">Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.</span><span class="sxs-lookup"><span data-stu-id="463f6-244">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="463f6-245">Отсутствие семантического разрыва с SQL.</span><span class="sxs-lookup"><span data-stu-id="463f6-245">No semantic gap with SQL.</span></span> <span data-ttu-id="463f6-246">XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.</span><span class="sxs-lookup"><span data-stu-id="463f6-246">ELinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="463f6-247">В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .</span><span class="sxs-lookup"><span data-stu-id="463f6-247">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="463f6-248">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="463f6-248">Website</span></span>](https://entitylinq.com/)

### <a name="ramses"></a><span data-ttu-id="463f6-249">Ramses</span><span class="sxs-lookup"><span data-stu-id="463f6-249">Ramses</span></span>

<span data-ttu-id="463f6-250">Перехватчики жизненного цикла (для SaveChanges).</span><span class="sxs-lookup"><span data-stu-id="463f6-250">Lifecycle hooks (for SaveChanges).</span></span> <span data-ttu-id="463f6-251">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-251">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="463f6-252">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-252">GitHub repository</span></span>](https://github.com/JValck/Ramses)

### <a name="efcorenamingconventions"></a><span data-ttu-id="463f6-253">EFCore.NamingConventions</span><span class="sxs-lookup"><span data-stu-id="463f6-253">EFCore.NamingConventions</span></span>

<span data-ttu-id="463f6-254">Автоматически преобразует имена всех таблиц и столбцов для использования "змеиного_регистра", ВСЕХ ПРОПИСНЫХ или всех строчных букв.</span><span class="sxs-lookup"><span data-stu-id="463f6-254">This will automatically make all your table and column names have snake_case, all UPPER or all lower case naming.</span></span> <span data-ttu-id="463f6-255">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-255">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-256">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-256">GitHub repository</span></span>](https://github.com/efcore/EFCore.NamingConventions)

### <a name="simplersoftwareentityframeworkcoresqlservernodatime"></a><span data-ttu-id="463f6-257">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span><span class="sxs-lookup"><span data-stu-id="463f6-257">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span></span>

<span data-ttu-id="463f6-258">Добавляет встроенную поддержку типов NodaTime в EntityFrameworkCore для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="463f6-258">Adds native support to EntityFrameworkCore for SQL Server for the NodaTime types.</span></span> <span data-ttu-id="463f6-259">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-259">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-260">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-260">GitHub repository</span></span>](https://github.com/StevenRasmussen/EFCore.SqlServer.NodaTime)

### <a name="dabbleentityframeworkcoretemporalquery"></a><span data-ttu-id="463f6-261">Dabble.EntityFrameworkCore.Temporal.Query</span><span class="sxs-lookup"><span data-stu-id="463f6-261">Dabble.EntityFrameworkCore.Temporal.Query</span></span>

<span data-ttu-id="463f6-262">LINQ-расширения Entity Framework Core 3.1 для поддержки запросов во временных таблицах Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="463f6-262">LINQ extensions to Entity Framework Core 3.1 to support Microsoft SQL Server Temporal Table Querying.</span></span> <span data-ttu-id="463f6-263">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-263">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-264">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-264">GitHub repository</span></span>](https://github.com/Adam-Langley/efcore-temporal-query)

### <a name="entityframeworkcoresqlserverhierarchyid"></a><span data-ttu-id="463f6-265">EntityFrameworkCore.SqlServer.HierarchyId</span><span class="sxs-lookup"><span data-stu-id="463f6-265">EntityFrameworkCore.SqlServer.HierarchyId</span></span>

<span data-ttu-id="463f6-266">Добавляет поддержку hierarchyid в поставщик SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="463f6-266">Adds hierarchyid support to the SQL Server EF Core provider.</span></span> <span data-ttu-id="463f6-267">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-267">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-268">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-268">GitHub repository</span></span>](https://github.com/efcore/EFCore.SqlServer.HierarchyId)

### <a name="linq2dbentityframeworkcore"></a><span data-ttu-id="463f6-269">linq2db.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="463f6-269">linq2db.EntityFrameworkCore</span></span>

<span data-ttu-id="463f6-270">Альтернативный переводчик запросов LINQ к выражениям SQL.</span><span class="sxs-lookup"><span data-stu-id="463f6-270">Alternative translator of LINQ queries to SQL expressions.</span></span> <span data-ttu-id="463f6-271">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-271">For EF Core: 3, 5.</span></span>

<span data-ttu-id="463f6-272">Включает поддержку расширенных функций SQL, таких как выражения CTE, массовое копирование, табличные указания, оконные функции, временные таблицы и операции создания, обновления и удаления на стороне базы данных.</span><span class="sxs-lookup"><span data-stu-id="463f6-272">Includes support for advanced SQL features such as CTEs, bulk copy, table hints, windowed functions, temporary tables, and database-side create/update/delete operations.</span></span>

[<span data-ttu-id="463f6-273">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-273">GitHub repository</span></span>](https://github.com/linq2db/linq2db.EntityFrameworkCore)

### <a name="efcoresoftdelete"></a><span data-ttu-id="463f6-274">EFCore.SoftDelete</span><span class="sxs-lookup"><span data-stu-id="463f6-274">EFCore.SoftDelete</span></span>

<span data-ttu-id="463f6-275">Реализация для обратимого удаления сущностей.</span><span class="sxs-lookup"><span data-stu-id="463f6-275">An implementation for soft deleting entities.</span></span> <span data-ttu-id="463f6-276">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-276">For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-277">NuGet</span><span class="sxs-lookup"><span data-stu-id="463f6-277">NuGet</span></span>](https://www.nuget.org/packages/EFCore.SoftDelete)

### <a name="entityframeworkcoreconfigurationmanager"></a><span data-ttu-id="463f6-278">EntityFrameworkCore.ConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="463f6-278">EntityFrameworkCore.ConfigurationManager</span></span>

<span data-ttu-id="463f6-279">Расширяет возможности EF Core для разрешения строк подключения из файла App.config. Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="463f6-279">Extends EF Core to resolve connection strings from App.config. For EF Core: 3.</span></span>

[<span data-ttu-id="463f6-280">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-280">GitHub repository</span></span>](https://github.com/efcore/EFCore.ConfigurationManager)

### <a name="detached-mapper"></a><span data-ttu-id="463f6-281">Detached Mapper</span><span class="sxs-lookup"><span data-stu-id="463f6-281">Detached Mapper</span></span>

<span data-ttu-id="463f6-282">Средство сопоставления DTO-Entity с обработкой композиции/агрегирования (аналогично GraphDiff).</span><span class="sxs-lookup"><span data-stu-id="463f6-282">A DTO-Entity mapper with composition/aggregation handling (similar to GraphDiff).</span></span> <span data-ttu-id="463f6-283">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-283">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-284">NuGet</span><span class="sxs-lookup"><span data-stu-id="463f6-284">NuGet</span></span>](https://www.nuget.org/packages/Detached.Mappers.EntityFramework)

### <a name="entityframeworkcoresqlitenodatime"></a><span data-ttu-id="463f6-285">EntityFrameworkCore.Sqlite.NodaTime</span><span class="sxs-lookup"><span data-stu-id="463f6-285">EntityFrameworkCore.Sqlite.NodaTime</span></span>

<span data-ttu-id="463f6-286">Добавляет поддержку типов [NodaTime](https://nodatime.org) при использовании [SQLite](https://sqlite.org).</span><span class="sxs-lookup"><span data-stu-id="463f6-286">Adds support for [NodaTime](https://nodatime.org) types when using [SQLite](https://sqlite.org).</span></span> <span data-ttu-id="463f6-287">Для EF Core: 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-287">For EF Core: 5.</span></span>

[<span data-ttu-id="463f6-288">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-288">GitHub repository</span></span>](https://github.com/khellang/EFCore.Sqlite.NodaTime)

### <a name="erikejentityframeworkcoresqlserverdacpac"></a><span data-ttu-id="463f6-289">ErikEJ.EntityFrameworkCore.SqlServer.Dacpac</span><span class="sxs-lookup"><span data-stu-id="463f6-289">ErikEJ.EntityFrameworkCore.SqlServer.Dacpac</span></span>

<span data-ttu-id="463f6-290">Обеспечивает реконструирование модели EF Core из пакета приложения уровня данных SQL Server (.dacpac).</span><span class="sxs-lookup"><span data-stu-id="463f6-290">Enables reverse engineering an EF Core model from a SQL Server data-tier application package (.dacpac).</span></span> <span data-ttu-id="463f6-291">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-291">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-292">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-292">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki/ErikEJ.EntityFrameworkCore.SqlServer.Dacpac)

### <a name="erikejentityframeworkcoredgmlbuilder"></a><span data-ttu-id="463f6-293">ErikEJ.EntityFrameworkCore.DgmlBuilder</span><span class="sxs-lookup"><span data-stu-id="463f6-293">ErikEJ.EntityFrameworkCore.DgmlBuilder</span></span>

<span data-ttu-id="463f6-294">Создает содержимое DGML (граф), которое визуализирует DbContext.</span><span class="sxs-lookup"><span data-stu-id="463f6-294">Generate DGML (Graph) content that visualizes your DbContext.</span></span> <span data-ttu-id="463f6-295">Добавляет метод расширения AsDgml() в класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="463f6-295">Adds the AsDgml() extension method to the DbContext class.</span></span> <span data-ttu-id="463f6-296">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-296">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-297">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-297">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki/Inspect-your-DbContext-model)

### <a name="entityframeworkexceptions"></a><span data-ttu-id="463f6-298">EntityFramework.Exceptions</span><span class="sxs-lookup"><span data-stu-id="463f6-298">EntityFramework.Exceptions</span></span>

<span data-ttu-id="463f6-299">В Entity Framework Core все исключения базы данных упаковываются в DbUpdateException.</span><span class="sxs-lookup"><span data-stu-id="463f6-299">When using Entity Framework Core all database exceptions are wrapped in DbUpdateException.</span></span> <span data-ttu-id="463f6-300">EntityFramework.Exceptions обрабатывает все сведения о базе данных, чтобы определить нарушенное ограничение, и позволяет применять типизированные исключения, например `UniqueConstraintException`, `CannotInsertNullException`, `MaxLengthExceededException`, `NumericOverflowException`, `ReferenceConstraintException`, если запрос нарушает ограничения базы данных.</span><span class="sxs-lookup"><span data-stu-id="463f6-300">EntityFramework.Exceptions handles all the database-specific details to find which constraint was violated and allows you to use typed exceptions such as `UniqueConstraintException`, `CannotInsertNullException`, `MaxLengthExceededException`, `NumericOverflowException`, `ReferenceConstraintException` when your query violates database constraints.</span></span>

<span data-ttu-id="463f6-301">Поддерживает SQL Server, Postgres, MySql, SQLite и Oracle.</span><span class="sxs-lookup"><span data-stu-id="463f6-301">Supports SQL Server, Postgres, MySql, SQLite and Oracle</span></span>

<span data-ttu-id="463f6-302">Для EF Core: 3, 5.</span><span class="sxs-lookup"><span data-stu-id="463f6-302">For EF Core: 3, 5.</span></span>

[<span data-ttu-id="463f6-303">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="463f6-303">GitHub Repository</span></span>](https://github.com/Giorgi/EntityFramework.Exceptions)
