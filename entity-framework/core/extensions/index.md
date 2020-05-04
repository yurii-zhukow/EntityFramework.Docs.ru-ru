---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 04/11/2020
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 246cf687f2a087e9a9a569c875b27712ebe80c5c
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82538335"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="b38e5-102">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="b38e5-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="b38e5-103">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="b38e5-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="b38e5-104">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="b38e5-105">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="b38e5-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="b38e5-106">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="b38e5-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="b38e5-107">Инструменты</span><span class="sxs-lookup"><span data-stu-id="b38e5-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="b38e5-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="b38e5-108">LLBLGen Pro</span></span>

<span data-ttu-id="b38e5-109">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="b38e5-110">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="b38e5-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="b38e5-111">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-111">For EF Core: 2, 3</span></span>

[<span data-ttu-id="b38e5-112">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="b38e5-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="b38e5-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="b38e5-113">Devart Entity Developer</span></span>

<span data-ttu-id="b38e5-114">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="b38e5-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="b38e5-115">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="b38e5-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="b38e5-116">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-116">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-117">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="b38e5-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="b38e5-118">Использование nHydrate ORM для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b38e5-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="b38e5-119">ORM создает строго типизированные расширяемые классы для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b38e5-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="b38e5-120">Созданный код представляет собой Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="b38e5-121">Различий при этом нет.</span><span class="sxs-lookup"><span data-stu-id="b38e5-121">There is no difference.</span></span> <span data-ttu-id="b38e5-122">Это не замена EF или пользовательского ORM.</span><span class="sxs-lookup"><span data-stu-id="b38e5-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="b38e5-123">Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных.</span><span class="sxs-lookup"><span data-stu-id="b38e5-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="b38e5-124">Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами.</span><span class="sxs-lookup"><span data-stu-id="b38e5-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="b38e5-125">Установщик отслеживает изменения модели и создает скрипты обновления.</span><span class="sxs-lookup"><span data-stu-id="b38e5-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="b38e5-126">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-126">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-127">Сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="b38e5-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="b38e5-128">EF Core Power Tools</span></span>

<span data-ttu-id="b38e5-129">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="b38e5-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="b38e5-130">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="b38e5-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="b38e5-131">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-132">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="b38e5-133">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b38e5-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="b38e5-134">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="b38e5-135">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="b38e5-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="b38e5-136">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="b38e5-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="b38e5-137">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-137">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="b38e5-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="b38e5-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="b38e5-139">CatFactory</span></span>

<span data-ttu-id="b38e5-140">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b38e5-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="b38e5-141">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-141">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-142">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="b38e5-143">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="b38e5-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="b38e5-144">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="b38e5-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="b38e5-145">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="b38e5-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="b38e5-146">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-146">For EF Core: 2.</span></span>

<span data-ttu-id="b38e5-147">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="b38e5-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="b38e5-148">Расширения</span><span class="sxs-lookup"><span data-stu-id="b38e5-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="b38e5-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="b38e5-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="b38e5-150">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="b38e5-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="b38e5-151">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-151">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-152">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efcoresecondlevelcacheinterceptor"></a><span data-ttu-id="b38e5-153">EFCoreSecondLevelCacheInterceptor</span><span class="sxs-lookup"><span data-stu-id="b38e5-153">EFCoreSecondLevelCacheInterceptor</span></span>

<span data-ttu-id="b38e5-154">Кэширование второго уровня — это кэширование запросов.</span><span class="sxs-lookup"><span data-stu-id="b38e5-154">Second level caching is a query cache.</span></span> <span data-ttu-id="b38e5-155">Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.</span><span class="sxs-lookup"><span data-stu-id="b38e5-155">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span> <span data-ttu-id="b38e5-156">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-156">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-157">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-157">GitHub repository</span></span>](https://github.com/VahidN/EFCoreSecondLevelCacheInterceptor)

### <a name="geco"></a><span data-ttu-id="b38e5-158">Geco</span><span class="sxs-lookup"><span data-stu-id="b38e5-158">Geco</span></span>

<span data-ttu-id="b38e5-159">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="b38e5-159">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="b38e5-160">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="b38e5-160">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="b38e5-161">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="b38e5-161">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="b38e5-162">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-162">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-163">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-163">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="b38e5-164">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="b38e5-164">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="b38e5-165">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="b38e5-165">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="b38e5-166">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-166">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-167">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-167">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="b38e5-168">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="b38e5-168">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="b38e5-169">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="b38e5-169">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="b38e5-170">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-170">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-171">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-171">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="b38e5-172">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="b38e5-172">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="b38e5-173">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="b38e5-173">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="b38e5-174">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-174">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-175">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-175">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="b38e5-176">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="b38e5-176">EFCore.BulkExtensions</span></span>

<span data-ttu-id="b38e5-177">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="b38e5-177">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="b38e5-178">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-178">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-179">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-179">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="b38e5-180">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="b38e5-180">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="b38e5-181">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="b38e5-181">Adds design-time pluralization.</span></span> <span data-ttu-id="b38e5-182">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-182">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-183">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-183">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="b38e5-184">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="b38e5-184">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="b38e5-185">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="b38e5-185">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="b38e5-186">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-186">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-187">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-187">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="b38e5-188">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="b38e5-188">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="b38e5-189">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-189">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="b38e5-190">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="b38e5-190">Makes it act more like a relational provider.</span></span> <span data-ttu-id="b38e5-191">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-191">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-192">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-192">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="b38e5-193">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="b38e5-193">EFCore.TemporalSupport</span></span>

<span data-ttu-id="b38e5-194">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="b38e5-194">An implementation of temporal support.</span></span> <span data-ttu-id="b38e5-195">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-195">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-196">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-196">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="b38e5-197">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="b38e5-197">EfCoreTemporalTable</span></span>

<span data-ttu-id="b38e5-198">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="b38e5-198">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="b38e5-199">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-199">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-200">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-200">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="b38e5-201">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="b38e5-201">EFCore.TimeTraveler</span></span>

<span data-ttu-id="b38e5-202">Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-202">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="b38e5-203">Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="b38e5-203">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="b38e5-204">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-204">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-205">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-205">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="b38e5-206">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="b38e5-206">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="b38e5-207">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="b38e5-207">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="b38e5-208">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-208">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-209">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-209">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="b38e5-210">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="b38e5-210">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="b38e5-211">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="b38e5-211">A high-performance second-level query cache.</span></span> <span data-ttu-id="b38e5-212">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b38e5-212">For EF Core: 2.</span></span>

[<span data-ttu-id="b38e5-213">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-213">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="b38e5-214">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="b38e5-214">Entity Framework Plus</span></span>

<span data-ttu-id="b38e5-215">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="b38e5-215">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="b38e5-216">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-216">For EF Core: 2, 3.</span></span>

<span data-ttu-id="b38e5-217">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="b38e5-217">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="b38e5-218">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b38e5-218">Entity Framework Extensions</span></span>

<span data-ttu-id="b38e5-219">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="b38e5-219">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="b38e5-220">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-220">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-221">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="b38e5-221">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="b38e5-222">Expressionify</span><span class="sxs-lookup"><span data-stu-id="b38e5-222">Expressionify</span></span>

<span data-ttu-id="b38e5-223">Добавляет поддержку вызова методов расширения в лямбда-выражениях LINQ.</span><span class="sxs-lookup"><span data-stu-id="b38e5-223">Add support for calling extension methods in LINQ lambdas.</span></span> <span data-ttu-id="b38e5-224">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-224">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-225">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-225">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="b38e5-226">XLinq</span><span class="sxs-lookup"><span data-stu-id="b38e5-226">XLinq</span></span>

<span data-ttu-id="b38e5-227">Технология языка интегрированных запросов (LINQ) для реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="b38e5-227">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="b38e5-228">Позволяет писать строго типизированные запросы на C#.</span><span class="sxs-lookup"><span data-stu-id="b38e5-228">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="b38e5-229">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-229">For EF Core: 3.</span></span>

- <span data-ttu-id="b38e5-230">Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.</span><span class="sxs-lookup"><span data-stu-id="b38e5-230">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="b38e5-231">Отсутствие семантического разрыва с SQL.</span><span class="sxs-lookup"><span data-stu-id="b38e5-231">No semantic gap with SQL.</span></span> <span data-ttu-id="b38e5-232">XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.</span><span class="sxs-lookup"><span data-stu-id="b38e5-232">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="b38e5-233">В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .</span><span class="sxs-lookup"><span data-stu-id="b38e5-233">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="b38e5-234">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="b38e5-234">Website</span></span>](http://xlinq.live/)

### <a name="ramses"></a><span data-ttu-id="b38e5-235">Ramses</span><span class="sxs-lookup"><span data-stu-id="b38e5-235">Ramses</span></span>

<span data-ttu-id="b38e5-236">Перехватчики жизненного цикла (для SaveChanges).</span><span class="sxs-lookup"><span data-stu-id="b38e5-236">Lifecycle hooks (for SaveChanges).</span></span> <span data-ttu-id="b38e5-237">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-237">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b38e5-238">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-238">GitHub repository</span></span>](https://github.com/JValck/Ramses)

### <a name="efcorenamingconventions"></a><span data-ttu-id="b38e5-239">EFCore.NamingConventions</span><span class="sxs-lookup"><span data-stu-id="b38e5-239">EFCore.NamingConventions</span></span>

<span data-ttu-id="b38e5-240">Автоматически преобразует имена всех таблиц и столбцов для использования "змеиного_регистра", ВСЕХ ПРОПИСНЫХ или всех строчных букв.</span><span class="sxs-lookup"><span data-stu-id="b38e5-240">This will automatically make all your table and column names have snake_case, all UPPER or all lower case naming.</span></span> <span data-ttu-id="b38e5-241">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-241">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-242">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-242">GitHub repository</span></span>](https://github.com/efcore/EFCore.NamingConventions)

### <a name="simplersoftwareentityframeworkcoresqlservernodatime"></a><span data-ttu-id="b38e5-243">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span><span class="sxs-lookup"><span data-stu-id="b38e5-243">SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime</span></span>

<span data-ttu-id="b38e5-244">Добавляет встроенную поддержку типов NodaTime в EntityFrameworkCore для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b38e5-244">Adds native support to EntityFrameworkCore for SQL Server for the NodaTime types.</span></span> <span data-ttu-id="b38e5-245">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-245">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-246">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-246">GitHub repository</span></span>](https://github.com/StevenRasmussen/EFCore.SqlServer.NodaTime)

### <a name="dabbleentityframeworkcoretemporalquery"></a><span data-ttu-id="b38e5-247">Dabble.EntityFrameworkCore.Temporal.Query</span><span class="sxs-lookup"><span data-stu-id="b38e5-247">Dabble.EntityFrameworkCore.Temporal.Query</span></span>

<span data-ttu-id="b38e5-248">LINQ-расширения Entity Framework Core 3.1 для поддержки запросов во временных таблицах Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b38e5-248">LINQ extensions to Entity Framework Core 3.1 to support Microsoft SQL Server Temporal Table Querying.</span></span> <span data-ttu-id="b38e5-249">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-249">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-250">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-250">GitHub repository</span></span>](https://github.com/Adam-Langley/efcore-temporal-query)

### <a name="entityframeworkcoresqlserverhierarchyid"></a><span data-ttu-id="b38e5-251">EntityFrameworkCore.SqlServer.HierarchyId</span><span class="sxs-lookup"><span data-stu-id="b38e5-251">EntityFrameworkCore.SqlServer.HierarchyId</span></span>

<span data-ttu-id="b38e5-252">Добавляет поддержку hierarchyid в поставщик SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="b38e5-252">Adds hierarchyid support to the SQL Server EF Core provider.</span></span> <span data-ttu-id="b38e5-253">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b38e5-253">For EF Core: 3.</span></span>

[<span data-ttu-id="b38e5-254">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="b38e5-254">GitHub repository</span></span>](https://github.com/efcore/EFCore.SqlServer.HierarchyId)
