---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 04/11/2020
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 5b7945bd24ac19a82b08f4d5709555cf53485f4c
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238259"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="fd1f4-102">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="fd1f4-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="fd1f4-103">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="fd1f4-104">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="fd1f4-105">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="fd1f4-106">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="fd1f4-107">Инструменты</span><span class="sxs-lookup"><span data-stu-id="fd1f4-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="fd1f4-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="fd1f4-108">LLBLGen Pro</span></span>

<span data-ttu-id="fd1f4-109">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="fd1f4-110">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="fd1f4-111">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-111">For EF Core: 2, 3</span></span>

[<span data-ttu-id="fd1f4-112">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="fd1f4-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="fd1f4-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="fd1f4-113">Devart Entity Developer</span></span>

<span data-ttu-id="fd1f4-114">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="fd1f4-115">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="fd1f4-116">Для EF Core: 1, 2, 3, 5.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-116">For EF Core: 1, 2, 3, 5.</span></span>

[<span data-ttu-id="fd1f4-117">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="fd1f4-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="fd1f4-118">Использование nHydrate ORM для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fd1f4-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="fd1f4-119">ORM создает строго типизированные расширяемые классы для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="fd1f4-120">Созданный код представляет собой Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="fd1f4-121">Различий при этом нет.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-121">There is no difference.</span></span> <span data-ttu-id="fd1f4-122">Это не замена EF или пользовательского ORM.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="fd1f4-123">Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="fd1f4-124">Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="fd1f4-125">Установщик отслеживает изменения модели и создает скрипты обновления.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="fd1f4-126">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-126">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-127">Сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="fd1f4-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="fd1f4-128">EF Core Power Tools</span></span>

<span data-ttu-id="fd1f4-129">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="fd1f4-130">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="fd1f4-131">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-132">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="fd1f4-133">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fd1f4-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="fd1f4-134">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="fd1f4-135">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="fd1f4-136">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="fd1f4-137">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-137">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="fd1f4-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="fd1f4-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="fd1f4-139">CatFactory</span></span>

<span data-ttu-id="fd1f4-140">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="fd1f4-141">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-141">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-142">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="fd1f4-143">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="fd1f4-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="fd1f4-144">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="fd1f4-145">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="fd1f4-146">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-146">For EF Core: 2.</span></span>

<span data-ttu-id="fd1f4-147">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="fd1f4-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="fd1f4-148">Расширения</span><span class="sxs-lookup"><span data-stu-id="fd1f4-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="fd1f4-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="fd1f4-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="fd1f4-150">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="fd1f4-151">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-151">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-152">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efcoresecondlevelcacheinterceptor"></a><span data-ttu-id="fd1f4-153">EFCoreSecondLevelCacheInterceptor</span><span class="sxs-lookup"><span data-stu-id="fd1f4-153">EFCoreSecondLevelCacheInterceptor</span></span>

<span data-ttu-id="fd1f4-154">Кэширование второго уровня — это кэширование запросов.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-154">Second level caching is a query cache.</span></span> <span data-ttu-id="fd1f4-155">Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-155">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span> <span data-ttu-id="fd1f4-156">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-156">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-157">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-157">GitHub repository</span></span>](https://github.com/VahidN/EFCoreSecondLevelCacheInterceptor)

### <a name="geco"></a><span data-ttu-id="fd1f4-158">Geco</span><span class="sxs-lookup"><span data-stu-id="fd1f4-158">Geco</span></span>

<span data-ttu-id="fd1f4-159">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-159">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="fd1f4-160">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-160">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="fd1f4-161">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-161">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="fd1f4-162">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-162">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-163">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-163">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="fd1f4-164">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="fd1f4-164">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="fd1f4-165">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-165">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="fd1f4-166">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-166">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-167">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-167">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="fd1f4-168">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="fd1f4-168">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="fd1f4-169">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-169">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="fd1f4-170">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-170">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-171">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-171">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="fd1f4-172">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="fd1f4-172">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="fd1f4-173">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-173">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="fd1f4-174">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-174">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-175">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-175">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="fd1f4-176">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="fd1f4-176">EFCore.BulkExtensions</span></span>

<span data-ttu-id="fd1f4-177">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="fd1f4-177">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="fd1f4-178">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-178">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-179">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-179">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="fd1f4-180">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="fd1f4-180">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="fd1f4-181">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-181">Adds design-time pluralization.</span></span> <span data-ttu-id="fd1f4-182">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-182">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-183">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-183">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="fd1f4-184">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="fd1f4-184">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="fd1f4-185">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="fd1f4-185">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="fd1f4-186">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-186">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-187">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-187">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="fd1f4-188">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="fd1f4-188">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="fd1f4-189">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-189">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="fd1f4-190">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-190">Makes it act more like a relational provider.</span></span> <span data-ttu-id="fd1f4-191">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-191">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-192">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-192">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="fd1f4-193">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="fd1f4-193">EFCore.TemporalSupport</span></span>

<span data-ttu-id="fd1f4-194">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-194">An implementation of temporal support.</span></span> <span data-ttu-id="fd1f4-195">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-195">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-196">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-196">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="fd1f4-197">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="fd1f4-197">EfCoreTemporalTable</span></span>

<span data-ttu-id="fd1f4-198">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-198">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="fd1f4-199">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-199">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-200">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-200">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="fd1f4-201">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="fd1f4-201">EFCore.TimeTraveler</span></span>

<span data-ttu-id="fd1f4-202">Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-202">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="fd1f4-203">Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-203">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="fd1f4-204">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-204">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-205">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-205">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="fd1f4-206">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="fd1f4-206">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="fd1f4-207">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-207">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="fd1f4-208">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-208">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-209">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-209">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="fd1f4-210">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="fd1f4-210">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="fd1f4-211">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-211">A high-performance second-level query cache.</span></span> <span data-ttu-id="fd1f4-212">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-212">For EF Core: 2.</span></span>

[<span data-ttu-id="fd1f4-213">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-213">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entityframeworkcorencache"></a><span data-ttu-id="fd1f4-214">EntityFrameworkCore.NCache</span><span class="sxs-lookup"><span data-stu-id="fd1f4-214">EntityFrameworkCore.NCache</span></span>

<span data-ttu-id="fd1f4-215">Поставщик Entity Framework Core NCache является распределенным поставщиком кэша второго уровня для кэширования результатов запросов.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-215">NCache Entity Framework Core Provider is a distributed second level cache provider for caching query results.</span></span> <span data-ttu-id="fd1f4-216">Распределенная архитектура NCache повышает уровень его масштабируемости и высокой доступности.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-216">The distributed architecture of NCache makes it more scalable and highly available.</span></span> <span data-ttu-id="fd1f4-217">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-217">For EF Core 2.</span></span>

[<span data-ttu-id="fd1f4-218">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="fd1f4-218">Website</span></span>](https://www.alachisoft.com/ncache/ef-core-cache.html)

### <a name="entity-framework-plus"></a><span data-ttu-id="fd1f4-219">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="fd1f4-219">Entity Framework Plus</span></span>

<span data-ttu-id="fd1f4-220">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-220">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="fd1f4-221">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-221">For EF Core: 2, 3.</span></span>

<span data-ttu-id="fd1f4-222">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="fd1f4-222">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="fd1f4-223">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fd1f4-223">Entity Framework Extensions</span></span>

<span data-ttu-id="fd1f4-224">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-224">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="fd1f4-225">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-225">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-226">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="fd1f4-226">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="fd1f4-227">Expressionify</span><span class="sxs-lookup"><span data-stu-id="fd1f4-227">Expressionify</span></span>

<span data-ttu-id="fd1f4-228">Добавляет поддержку вызова методов расширения в лямбда-выражениях LINQ.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-228">Add support for calling extension methods in LINQ lambdas.</span></span> <span data-ttu-id="fd1f4-229">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-229">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-230">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-230">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="fd1f4-231">XLinq</span><span class="sxs-lookup"><span data-stu-id="fd1f4-231">XLinq</span></span>

<span data-ttu-id="fd1f4-232">Технология языка интегрированных запросов (LINQ) для реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-232">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="fd1f4-233">Позволяет писать строго типизированные запросы на C#.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-233">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="fd1f4-234">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-234">For EF Core: 3.</span></span>

- <span data-ttu-id="fd1f4-235">Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-235">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="fd1f4-236">Отсутствие семантического разрыва с SQL.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-236">No semantic gap with SQL.</span></span> <span data-ttu-id="fd1f4-237">XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-237">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="fd1f4-238">В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .</span><span class="sxs-lookup"><span data-stu-id="fd1f4-238">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="fd1f4-239">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="fd1f4-239">Website</span></span>](http://xlinq.live/)

### <a name="ramses"></a><span data-ttu-id="fd1f4-240">Ramses</span><span class="sxs-lookup"><span data-stu-id="fd1f4-240">Ramses</span></span>

<span data-ttu-id="fd1f4-241">Перехватчики жизненного цикла (для SaveChanges).</span><span class="sxs-lookup"><span data-stu-id="fd1f4-241">Lifecycle hooks (for SaveChanges).</span></span> <span data-ttu-id="fd1f4-242">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-242">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="fd1f4-243">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-243">GitHub repository</span></span>](https://github.com/JValck/Ramses)

### <a name="efcorenamingconventions"></a><span data-ttu-id="fd1f4-244">EFCore.NamingConventions</span><span class="sxs-lookup"><span data-stu-id="fd1f4-244">EFCore.NamingConventions</span></span>

<span data-ttu-id="fd1f4-245">Автоматически преобразует имена всех таблиц и столбцов для использования "змеиного_регистра", ВСЕХ ПРОПИСНЫХ или всех строчных букв.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-245">This will automatically make all your table and column names have snake_case, all UPPER or all lower case naming.</span></span> <span data-ttu-id="fd1f4-246">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-246">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-247">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-247">GitHub repository</span></span>](https://github.com/efcore/EFCore.NamingConventions)

### <a name="simplersoftwareentityframeworkcoresqlservernodatime"></a><span data-ttu-id="fd1f4-248">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span><span class="sxs-lookup"><span data-stu-id="fd1f4-248">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span></span>

<span data-ttu-id="fd1f4-249">Добавляет встроенную поддержку типов NodaTime в EntityFrameworkCore для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-249">Adds native support to EntityFrameworkCore for SQL Server for the NodaTime types.</span></span> <span data-ttu-id="fd1f4-250">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-250">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-251">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-251">GitHub repository</span></span>](https://github.com/StevenRasmussen/EFCore.SqlServer.NodaTime)

### <a name="dabbleentityframeworkcoretemporalquery"></a><span data-ttu-id="fd1f4-252">Dabble.EntityFrameworkCore.Temporal.Query</span><span class="sxs-lookup"><span data-stu-id="fd1f4-252">Dabble.EntityFrameworkCore.Temporal.Query</span></span>

<span data-ttu-id="fd1f4-253">LINQ-расширения Entity Framework Core 3.1 для поддержки запросов во временных таблицах Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-253">LINQ extensions to Entity Framework Core 3.1 to support Microsoft SQL Server Temporal Table Querying.</span></span> <span data-ttu-id="fd1f4-254">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-254">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-255">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-255">GitHub repository</span></span>](https://github.com/Adam-Langley/efcore-temporal-query)

### <a name="entityframeworkcoresqlserverhierarchyid"></a><span data-ttu-id="fd1f4-256">EntityFrameworkCore.SqlServer.HierarchyId</span><span class="sxs-lookup"><span data-stu-id="fd1f4-256">EntityFrameworkCore.SqlServer.HierarchyId</span></span>

<span data-ttu-id="fd1f4-257">Добавляет поддержку hierarchyid в поставщик SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-257">Adds hierarchyid support to the SQL Server EF Core provider.</span></span> <span data-ttu-id="fd1f4-258">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-258">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-259">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-259">GitHub repository</span></span>](https://github.com/efcore/EFCore.SqlServer.HierarchyId)

### <a name="linq2dbentityframeworkcore"></a><span data-ttu-id="fd1f4-260">linq2db.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="fd1f4-260">linq2db.EntityFrameworkCore</span></span>

<span data-ttu-id="fd1f4-261">Альтернативный переводчик запросов LINQ к выражениям SQL.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-261">Alternative translator of LINQ queries to SQL expressions.</span></span> <span data-ttu-id="fd1f4-262">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-262">For EF Core: 3.</span></span>

<span data-ttu-id="fd1f4-263">Включает поддержку расширенных функций SQL, таких как выражения CTE, массовое копирование, табличные указания, оконные функции, временные таблицы и операции создания, обновления и удаления на стороне базы данных.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-263">Includes support for advanced SQL features such as CTEs, bulk copy, table hints, windowed functions, temporary tables, and database-side create/update/delete operations.</span></span>

[<span data-ttu-id="fd1f4-264">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="fd1f4-264">GitHub repository</span></span>](https://github.com/linq2db/linq2db.EntityFrameworkCore)

### <a name="efcoresoftdelete"></a><span data-ttu-id="fd1f4-265">EFCore.SoftDelete</span><span class="sxs-lookup"><span data-stu-id="fd1f4-265">EFCore.SoftDelete</span></span>

<span data-ttu-id="fd1f4-266">Реализация для обратимого удаления сущностей.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-266">An implementation for soft deleting entities.</span></span> <span data-ttu-id="fd1f4-267">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="fd1f4-267">For EF Core: 3.</span></span>

[<span data-ttu-id="fd1f4-268">NuGet</span><span class="sxs-lookup"><span data-stu-id="fd1f4-268">NuGet</span></span>](https://www.nuget.org/packages/EFCore.SoftDelete)
