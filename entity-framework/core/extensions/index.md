---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 04/11/2020
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: f450742b761ef3daf35e32cf87c63a8ee8a7b8c3
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526411"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="d5535-102">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="d5535-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="d5535-103">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d5535-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d5535-104">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="d5535-105">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="d5535-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="d5535-106">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="d5535-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="d5535-107">Инструменты</span><span class="sxs-lookup"><span data-stu-id="d5535-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="d5535-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="d5535-108">LLBLGen Pro</span></span>

<span data-ttu-id="d5535-109">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="d5535-110">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="d5535-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="d5535-111">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-111">For EF Core: 2, 3</span></span>

[<span data-ttu-id="d5535-112">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="d5535-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="d5535-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="d5535-113">Devart Entity Developer</span></span>

<span data-ttu-id="d5535-114">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="d5535-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="d5535-115">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d5535-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="d5535-116">Для EF Core: 1, 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="d5535-116">For EF Core: 1, 2, 3, 5.</span></span>

[<span data-ttu-id="d5535-117">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="d5535-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="d5535-118">Использование nHydrate ORM для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d5535-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="d5535-119">ORM создает строго типизированные расширяемые классы для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d5535-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="d5535-120">Созданный код представляет собой Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="d5535-121">Различий при этом нет.</span><span class="sxs-lookup"><span data-stu-id="d5535-121">There is no difference.</span></span> <span data-ttu-id="d5535-122">Это не замена EF или пользовательского ORM.</span><span class="sxs-lookup"><span data-stu-id="d5535-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="d5535-123">Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных.</span><span class="sxs-lookup"><span data-stu-id="d5535-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="d5535-124">Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами.</span><span class="sxs-lookup"><span data-stu-id="d5535-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="d5535-125">Установщик отслеживает изменения модели и создает скрипты обновления.</span><span class="sxs-lookup"><span data-stu-id="d5535-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="d5535-126">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-126">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-127">Сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="d5535-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="d5535-128">EF Core Power Tools</span></span>

<span data-ttu-id="d5535-129">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="d5535-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="d5535-130">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="d5535-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="d5535-131">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-132">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="d5535-133">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d5535-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="d5535-134">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="d5535-135">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="d5535-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="d5535-136">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="d5535-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="d5535-137">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-137">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="d5535-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="d5535-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="d5535-139">CatFactory</span></span>

<span data-ttu-id="d5535-140">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5535-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="d5535-141">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-141">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-142">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="d5535-143">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="d5535-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="d5535-144">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="d5535-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="d5535-145">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="d5535-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="d5535-146">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-146">For EF Core: 2.</span></span>

<span data-ttu-id="d5535-147">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="d5535-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="d5535-148">Расширения</span><span class="sxs-lookup"><span data-stu-id="d5535-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="d5535-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="d5535-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="d5535-150">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="d5535-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="d5535-151">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-151">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-152">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efcoresecondlevelcacheinterceptor"></a><span data-ttu-id="d5535-153">EFCoreSecondLevelCacheInterceptor</span><span class="sxs-lookup"><span data-stu-id="d5535-153">EFCoreSecondLevelCacheInterceptor</span></span>

<span data-ttu-id="d5535-154">Кэширование второго уровня — это кэширование запросов.</span><span class="sxs-lookup"><span data-stu-id="d5535-154">Second level caching is a query cache.</span></span> <span data-ttu-id="d5535-155">Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.</span><span class="sxs-lookup"><span data-stu-id="d5535-155">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span> <span data-ttu-id="d5535-156">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-156">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-157">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-157">GitHub repository</span></span>](https://github.com/VahidN/EFCoreSecondLevelCacheInterceptor)

### <a name="geco"></a><span data-ttu-id="d5535-158">Geco</span><span class="sxs-lookup"><span data-stu-id="d5535-158">Geco</span></span>

<span data-ttu-id="d5535-159">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="d5535-159">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="d5535-160">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d5535-160">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="d5535-161">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="d5535-161">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="d5535-162">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-162">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-163">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-163">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="d5535-164">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="d5535-164">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="d5535-165">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="d5535-165">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="d5535-166">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-166">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-167">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-167">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="d5535-168">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="d5535-168">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="d5535-169">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="d5535-169">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="d5535-170">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-170">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-171">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-171">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="d5535-172">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="d5535-172">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="d5535-173">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="d5535-173">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="d5535-174">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-174">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-175">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-175">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="d5535-176">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="d5535-176">EFCore.BulkExtensions</span></span>

<span data-ttu-id="d5535-177">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="d5535-177">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="d5535-178">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-178">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-179">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-179">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="d5535-180">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="d5535-180">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="d5535-181">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="d5535-181">Adds design-time pluralization.</span></span> <span data-ttu-id="d5535-182">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-182">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-183">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-183">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="d5535-184">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="d5535-184">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="d5535-185">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="d5535-185">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="d5535-186">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-186">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-187">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-187">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="d5535-188">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="d5535-188">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="d5535-189">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-189">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="d5535-190">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="d5535-190">Makes it act more like a relational provider.</span></span> <span data-ttu-id="d5535-191">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-191">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-192">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-192">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="d5535-193">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="d5535-193">EFCore.TemporalSupport</span></span>

<span data-ttu-id="d5535-194">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="d5535-194">An implementation of temporal support.</span></span> <span data-ttu-id="d5535-195">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-195">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-196">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-196">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="d5535-197">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="d5535-197">EfCoreTemporalTable</span></span>

<span data-ttu-id="d5535-198">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="d5535-198">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="d5535-199">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-199">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-200">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-200">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="d5535-201">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="d5535-201">EFCore.TimeTraveler</span></span>

<span data-ttu-id="d5535-202">Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-202">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="d5535-203">Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="d5535-203">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="d5535-204">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-204">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-205">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-205">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="d5535-206">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="d5535-206">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="d5535-207">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="d5535-207">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="d5535-208">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-208">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-209">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-209">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="d5535-210">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="d5535-210">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="d5535-211">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="d5535-211">A high-performance second-level query cache.</span></span> <span data-ttu-id="d5535-212">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-212">For EF Core: 2.</span></span>

[<span data-ttu-id="d5535-213">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-213">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entityframeworkcorencache"></a><span data-ttu-id="d5535-214">EntityFrameworkCore.NCache</span><span class="sxs-lookup"><span data-stu-id="d5535-214">EntityFrameworkCore.NCache</span></span>

<span data-ttu-id="d5535-215">Поставщик Entity Framework Core NCache является распределенным поставщиком кэша второго уровня для кэширования результатов запросов.</span><span class="sxs-lookup"><span data-stu-id="d5535-215">NCache Entity Framework Core Provider is a distributed second level cache provider for caching query results.</span></span> <span data-ttu-id="d5535-216">Распределенная архитектура NCache повышает уровень его масштабируемости и высокой доступности.</span><span class="sxs-lookup"><span data-stu-id="d5535-216">The distributed architecture of NCache makes it more scalable and highly available.</span></span> <span data-ttu-id="d5535-217">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="d5535-217">For EF Core 2.</span></span>

[<span data-ttu-id="d5535-218">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="d5535-218">Website</span></span>](https://www.alachisoft.com/ncache/ef-core-cache.html)

### <a name="entity-framework-plus"></a><span data-ttu-id="d5535-219">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="d5535-219">Entity Framework Plus</span></span>

<span data-ttu-id="d5535-220">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="d5535-220">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="d5535-221">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-221">For EF Core: 2, 3.</span></span>

<span data-ttu-id="d5535-222">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="d5535-222">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="d5535-223">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d5535-223">Entity Framework Extensions</span></span>

<span data-ttu-id="d5535-224">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="d5535-224">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="d5535-225">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-225">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-226">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="d5535-226">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="d5535-227">Expressionify</span><span class="sxs-lookup"><span data-stu-id="d5535-227">Expressionify</span></span>

<span data-ttu-id="d5535-228">Добавляет поддержку вызова методов расширения в лямбда-выражениях LINQ.</span><span class="sxs-lookup"><span data-stu-id="d5535-228">Add support for calling extension methods in LINQ lambdas.</span></span> <span data-ttu-id="d5535-229">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-229">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-230">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-230">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="d5535-231">XLinq</span><span class="sxs-lookup"><span data-stu-id="d5535-231">XLinq</span></span>

<span data-ttu-id="d5535-232">Технология языка интегрированных запросов (LINQ) для реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="d5535-232">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="d5535-233">Позволяет писать строго типизированные запросы на C#.</span><span class="sxs-lookup"><span data-stu-id="d5535-233">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="d5535-234">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-234">For EF Core: 3.</span></span>

- <span data-ttu-id="d5535-235">Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.</span><span class="sxs-lookup"><span data-stu-id="d5535-235">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="d5535-236">Отсутствие семантического разрыва с SQL.</span><span class="sxs-lookup"><span data-stu-id="d5535-236">No semantic gap with SQL.</span></span> <span data-ttu-id="d5535-237">XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.</span><span class="sxs-lookup"><span data-stu-id="d5535-237">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="d5535-238">В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .</span><span class="sxs-lookup"><span data-stu-id="d5535-238">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="d5535-239">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="d5535-239">Website</span></span>](http://xlinq.live/)

### <a name="ramses"></a><span data-ttu-id="d5535-240">Ramses</span><span class="sxs-lookup"><span data-stu-id="d5535-240">Ramses</span></span>

<span data-ttu-id="d5535-241">Перехватчики жизненного цикла (для SaveChanges).</span><span class="sxs-lookup"><span data-stu-id="d5535-241">Lifecycle hooks (for SaveChanges).</span></span> <span data-ttu-id="d5535-242">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-242">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="d5535-243">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-243">GitHub repository</span></span>](https://github.com/JValck/Ramses)

### <a name="efcorenamingconventions"></a><span data-ttu-id="d5535-244">EFCore.NamingConventions</span><span class="sxs-lookup"><span data-stu-id="d5535-244">EFCore.NamingConventions</span></span>

<span data-ttu-id="d5535-245">Автоматически преобразует имена всех таблиц и столбцов для использования "змеиного_регистра", ВСЕХ ПРОПИСНЫХ или всех строчных букв.</span><span class="sxs-lookup"><span data-stu-id="d5535-245">This will automatically make all your table and column names have snake_case, all UPPER or all lower case naming.</span></span> <span data-ttu-id="d5535-246">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-246">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-247">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-247">GitHub repository</span></span>](https://github.com/efcore/EFCore.NamingConventions)

### <a name="simplersoftwareentityframeworkcoresqlservernodatime"></a><span data-ttu-id="d5535-248">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span><span class="sxs-lookup"><span data-stu-id="d5535-248">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span></span>

<span data-ttu-id="d5535-249">Добавляет встроенную поддержку типов NodaTime в EntityFrameworkCore для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5535-249">Adds native support to EntityFrameworkCore for SQL Server for the NodaTime types.</span></span> <span data-ttu-id="d5535-250">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-250">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-251">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-251">GitHub repository</span></span>](https://github.com/StevenRasmussen/EFCore.SqlServer.NodaTime)

### <a name="dabbleentityframeworkcoretemporalquery"></a><span data-ttu-id="d5535-252">Dabble.EntityFrameworkCore.Temporal.Query</span><span class="sxs-lookup"><span data-stu-id="d5535-252">Dabble.EntityFrameworkCore.Temporal.Query</span></span>

<span data-ttu-id="d5535-253">LINQ-расширения Entity Framework Core 3.1 для поддержки запросов во временных таблицах Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5535-253">LINQ extensions to Entity Framework Core 3.1 to support Microsoft SQL Server Temporal Table Querying.</span></span> <span data-ttu-id="d5535-254">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-254">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-255">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-255">GitHub repository</span></span>](https://github.com/Adam-Langley/efcore-temporal-query)

### <a name="entityframeworkcoresqlserverhierarchyid"></a><span data-ttu-id="d5535-256">EntityFrameworkCore.SqlServer.HierarchyId</span><span class="sxs-lookup"><span data-stu-id="d5535-256">EntityFrameworkCore.SqlServer.HierarchyId</span></span>

<span data-ttu-id="d5535-257">Добавляет поддержку hierarchyid в поставщик SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5535-257">Adds hierarchyid support to the SQL Server EF Core provider.</span></span> <span data-ttu-id="d5535-258">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-258">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-259">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-259">GitHub repository</span></span>](https://github.com/efcore/EFCore.SqlServer.HierarchyId)

### <a name="linq2dbentityframeworkcore"></a><span data-ttu-id="d5535-260">linq2db.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="d5535-260">linq2db.EntityFrameworkCore</span></span>

<span data-ttu-id="d5535-261">Альтернативный переводчик запросов LINQ к выражениям SQL.</span><span class="sxs-lookup"><span data-stu-id="d5535-261">Alternative translator of LINQ queries to SQL expressions.</span></span> <span data-ttu-id="d5535-262">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-262">For EF Core: 3.</span></span>

<span data-ttu-id="d5535-263">Включает поддержку расширенных функций SQL, таких как выражения CTE, массовое копирование, табличные указания, оконные функции, временные таблицы и операции создания, обновления и удаления на стороне базы данных.</span><span class="sxs-lookup"><span data-stu-id="d5535-263">Includes support for advanced SQL features such as CTEs, bulk copy, table hints, windowed functions, temporary tables, and database-side create/update/delete operations.</span></span>

[<span data-ttu-id="d5535-264">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="d5535-264">GitHub repository</span></span>](https://github.com/linq2db/linq2db.EntityFrameworkCore)

### <a name="efcoresoftdelete"></a><span data-ttu-id="d5535-265">EFCore.SoftDelete</span><span class="sxs-lookup"><span data-stu-id="d5535-265">EFCore.SoftDelete</span></span>

<span data-ttu-id="d5535-266">Реализация для обратимого удаления сущностей.</span><span class="sxs-lookup"><span data-stu-id="d5535-266">An implementation for soft deleting entities.</span></span> <span data-ttu-id="d5535-267">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="d5535-267">For EF Core: 3.</span></span>

[<span data-ttu-id="d5535-268">NuGet</span><span class="sxs-lookup"><span data-stu-id="d5535-268">NuGet</span></span>](https://www.nuget.org/packages/EFCore.SoftDelete)
