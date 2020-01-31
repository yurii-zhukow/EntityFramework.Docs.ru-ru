---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 99f59153a452a2f4aad5811110ebc5b5da7717ef
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888041"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="0fc2e-102">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="0fc2e-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="0fc2e-103">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0fc2e-104">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="0fc2e-105">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="0fc2e-106">В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="0fc2e-107">Инструменты</span><span class="sxs-lookup"><span data-stu-id="0fc2e-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="0fc2e-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="0fc2e-108">LLBLGen Pro</span></span>

<span data-ttu-id="0fc2e-109">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="0fc2e-110">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="0fc2e-111">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-111">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-112">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="0fc2e-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="0fc2e-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="0fc2e-113">Devart Entity Developer</span></span>

<span data-ttu-id="0fc2e-114">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="0fc2e-115">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="0fc2e-116">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-116">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-117">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="0fc2e-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="0fc2e-118">Использование nHydrate ORM для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0fc2e-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="0fc2e-119">ORM создает строго типизированные расширяемые классы для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="0fc2e-120">Созданный код представляет собой Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="0fc2e-121">Различий при этом нет.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-121">There is no difference.</span></span> <span data-ttu-id="0fc2e-122">Это не замена EF или пользовательского ORM.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="0fc2e-123">Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="0fc2e-124">Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="0fc2e-125">Установщик отслеживает изменения модели и создает скрипты обновления.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="0fc2e-126">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-126">For EF Core: 3.</span></span>

[<span data-ttu-id="0fc2e-127">Сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="0fc2e-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="0fc2e-128">EF Core Power Tools</span></span>

<span data-ttu-id="0fc2e-129">EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="0fc2e-130">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="0fc2e-131">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="0fc2e-132">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="0fc2e-133">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0fc2e-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="0fc2e-134">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="0fc2e-135">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="0fc2e-136">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="0fc2e-137">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-137">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="0fc2e-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="0fc2e-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="0fc2e-139">CatFactory</span></span>

<span data-ttu-id="0fc2e-140">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="0fc2e-141">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-141">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-142">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="0fc2e-143">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="0fc2e-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="0fc2e-144">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="0fc2e-145">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="0fc2e-146">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-146">For EF Core: 2.</span></span>

<span data-ttu-id="0fc2e-147">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="0fc2e-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="0fc2e-148">Расширения</span><span class="sxs-lookup"><span data-stu-id="0fc2e-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="0fc2e-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="0fc2e-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="0fc2e-150">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="0fc2e-151">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-151">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-152">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="0fc2e-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="0fc2e-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="0fc2e-154">Расширение, которое позволяет хранить результаты запросов EF Core в кэше второго уровня, чтобы при последующем выполнении тех же запросов не обращаться к базе данных и получить данные напрямую из кэша.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="0fc2e-155">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-155">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-156">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="0fc2e-157">Geco</span><span class="sxs-lookup"><span data-stu-id="0fc2e-157">Geco</span></span>

<span data-ttu-id="0fc2e-158">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="0fc2e-159">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="0fc2e-160">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="0fc2e-161">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-161">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-162">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="0fc2e-163">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="0fc2e-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="0fc2e-164">Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="0fc2e-165">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="0fc2e-166">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="0fc2e-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0fc2e-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="0fc2e-168">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="0fc2e-169">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="0fc2e-170">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="0fc2e-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="0fc2e-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="0fc2e-172">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="0fc2e-173">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-173">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-174">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="0fc2e-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="0fc2e-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="0fc2e-176">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="0fc2e-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="0fc2e-177">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="0fc2e-178">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="0fc2e-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="0fc2e-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="0fc2e-180">Добавляет преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-180">Adds design-time pluralization.</span></span> <span data-ttu-id="0fc2e-181">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-181">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-182">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="0fc2e-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="0fc2e-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="0fc2e-184">Новая реализация атрибута [Index] (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="0fc2e-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="0fc2e-185">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="0fc2e-186">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="0fc2e-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="0fc2e-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="0fc2e-188">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="0fc2e-189">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="0fc2e-190">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-190">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-191">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="0fc2e-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="0fc2e-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="0fc2e-193">Реализация поддержки темпоральных операций.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-193">An implementation of temporal support.</span></span> <span data-ttu-id="0fc2e-194">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-194">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-195">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="0fc2e-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="0fc2e-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="0fc2e-197">Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="0fc2e-198">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-198">For EF Core: 3.</span></span>

[<span data-ttu-id="0fc2e-199">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="0fc2e-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="0fc2e-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="0fc2e-201">Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="0fc2e-202">Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="0fc2e-203">Для EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-203">For EF Core: 3.</span></span>

[<span data-ttu-id="0fc2e-204">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="0fc2e-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="0fc2e-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="0fc2e-206">Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="0fc2e-207">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-207">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-208">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="0fc2e-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="0fc2e-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="0fc2e-210">Высокопроизводительный кэш запросов второго уровня.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="0fc2e-211">Для EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-211">For EF Core: 2.</span></span>

[<span data-ttu-id="0fc2e-212">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="0fc2e-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="0fc2e-213">Entity Framework Plus</span></span>

<span data-ttu-id="0fc2e-214">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="0fc2e-215">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="0fc2e-216">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="0fc2e-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="0fc2e-217">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0fc2e-217">Entity Framework Extensions</span></span>

<span data-ttu-id="0fc2e-218">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="0fc2e-219">Для EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="0fc2e-220">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="0fc2e-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="0fc2e-221">Expressionify</span><span class="sxs-lookup"><span data-stu-id="0fc2e-221">Expressionify</span></span>

<span data-ttu-id="0fc2e-222">Позволяет вызывать методы расширения в лямбда-выражениях LINQ.</span><span class="sxs-lookup"><span data-stu-id="0fc2e-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="0fc2e-223">Для EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="0fc2e-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="0fc2e-224">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="0fc2e-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)
