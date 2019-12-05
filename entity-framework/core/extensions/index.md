---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 86befa151adc8278ff8c76bdef023ca26a12508b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824626"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="1c2bb-102">Инструменты и расширения EF Core</span><span class="sxs-lookup"><span data-stu-id="1c2bb-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="1c2bb-103">Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="1c2bb-104">Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="1c2bb-105">Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="1c2bb-106">Инструменты</span><span class="sxs-lookup"><span data-stu-id="1c2bb-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="1c2bb-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="1c2bb-107">LLBLGen Pro</span></span>

<span data-ttu-id="1c2bb-108">LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="1c2bb-109">Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="1c2bb-110">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="1c2bb-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="1c2bb-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="1c2bb-111">Devart Entity Developer</span></span>

<span data-ttu-id="1c2bb-112">Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="1c2bb-113">Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span>

[<span data-ttu-id="1c2bb-114">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="1c2bb-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="1c2bb-115">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="1c2bb-115">EF Core Power Tools</span></span>

<span data-ttu-id="1c2bb-116">EF Core Power Tools — это расширение Visual Studio 2017, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="1c2bb-117">Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="1c2bb-118">Вики-сайт GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="1c2bb-119">Редактор Visual Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1c2bb-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="1c2bb-120">Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="1c2bb-121">Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="1c2bb-122">Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="1c2bb-123">Marketplace</span><span class="sxs-lookup"><span data-stu-id="1c2bb-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="1c2bb-124">CatFactory</span><span class="sxs-lookup"><span data-stu-id="1c2bb-124">CatFactory</span></span>

<span data-ttu-id="1c2bb-125">CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="1c2bb-126">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="1c2bb-127">Entity Framework Core Generator от LoreSoft</span><span class="sxs-lookup"><span data-stu-id="1c2bb-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="1c2bb-128">Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="1c2bb-129">Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-129">This tool supports generating view models, validation, and object mapper code.</span></span>

<span data-ttu-id="1c2bb-130">[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="1c2bb-130">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="1c2bb-131">Расширения</span><span class="sxs-lookup"><span data-stu-id="1c2bb-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="1c2bb-132">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="1c2bb-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="1c2bb-133">Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="1c2bb-134">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="1c2bb-135">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="1c2bb-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="1c2bb-136">Портированная версия .NET Core/.NET Standard для System.Linq.Dynamic, которая включает поддержку асинхронных операций в EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="1c2bb-137">Расширение System.Linq.Dynamic изначально представляло собой пример Майкрософт, который демонстрировал динамическое создание запросов LINQ из строковых выражений, а не из кода.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="1c2bb-138">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="1c2bb-139">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="1c2bb-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="1c2bb-140">Расширение, которое позволяет хранить результаты запросов EF Core в кэше второго уровня, чтобы при последующем выполнении тех же запросов не обращаться к базе данных и получить данные напрямую из кэша.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="1c2bb-141">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="1c2bb-142">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="1c2bb-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="1c2bb-143">Эта библиотека позволяет получить значения первичного ключа (включая составные ключи) из любой сущности в виде словаря.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="1c2bb-144">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="1c2bb-145">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="1c2bb-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="1c2bb-146">Эта библиотека обеспечивает строго типизированный доступ к исходным значениям свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-146">This library enables strongly typed access to the original values of entity properties.</span></span>

[<span data-ttu-id="1c2bb-147">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="1c2bb-148">Geco</span><span class="sxs-lookup"><span data-stu-id="1c2bb-148">Geco</span></span>

<span data-ttu-id="1c2bb-149">Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="1c2bb-150">Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="1c2bb-151">Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="1c2bb-152">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="1c2bb-153">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1c2bb-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="1c2bb-154">LinqKit.Microsoft.EntityFrameworkCore представляет собой версию библиотеки LINQKit, совместимую с EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="1c2bb-155">LINQKit — это бесплатный набор расширений для опытных пользователей LINQ to SQL и Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="1c2bb-156">Он включает дополнительные функциональные возможности, такие как динамическое создание выражений предикатов и использование переменных выражений во вложенных запросах.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="1c2bb-157">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="1c2bb-158">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1c2bb-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="1c2bb-159">NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="1c2bb-160">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="1c2bb-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="1c2bb-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="1c2bb-162">Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="1c2bb-163">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="1c2bb-164">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="1c2bb-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="1c2bb-165">Расширения EF Core для массовых операций (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="1c2bb-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="1c2bb-166">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="1c2bb-167">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="1c2bb-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="1c2bb-168">Добавляет в EF Core преобразование во множественную форму для времени разработки.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="1c2bb-169">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="1c2bb-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span><span class="sxs-lookup"><span data-stu-id="1c2bb-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="1c2bb-171">В простых сценариях для формирования заданного запроса LINQ может использоваться простой метод расширения, который получает инструкцию SQL в EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="1c2bb-172">Использование метода ToSql ограничено простыми сценариями, так как EF Core может создавать несколько инструкций SQL в одном запросе LINQ, а также различные инструкции SQL, которые зависят от значений параметров.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="1c2bb-173">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="1c2bb-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="1c2bb-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="1c2bb-175">Новая реализация атрибута [Index] для EF Core (с расширением для создания моделей).</span><span class="sxs-lookup"><span data-stu-id="1c2bb-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="1c2bb-176">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="1c2bb-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="1c2bb-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="1c2bb-178">Предоставляет оболочку для поставщика базы данных в памяти EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="1c2bb-179">Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="1c2bb-180">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="1c2bb-181">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="1c2bb-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="1c2bb-182">Реализация поддержки темпоральных операций для EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="1c2bb-183">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="1c2bb-184">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="1c2bb-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="1c2bb-185">Высокопроизводительный кэш запросов второго уровня для EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="1c2bb-186">Репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="1c2bb-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="1c2bb-187">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="1c2bb-187">Entity Framework Plus</span></span>

<span data-ttu-id="1c2bb-188">Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-188">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span>

<span data-ttu-id="1c2bb-189">[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="1c2bb-189">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="1c2bb-190">Расширения Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1c2bb-190">Entity Framework Extensions</span></span>

<span data-ttu-id="1c2bb-191">Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других.</span><span class="sxs-lookup"><span data-stu-id="1c2bb-191">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span>

[<span data-ttu-id="1c2bb-192">Веб-сайт</span><span class="sxs-lookup"><span data-stu-id="1c2bb-192">Website</span></span>](https://entityframework-extensions.net/)
