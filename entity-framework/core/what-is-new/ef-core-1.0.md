---
title: Новые возможности в Core EF 1.0 (EF Core)
description: Изменения и улучшения в Entity Framework Core 1.0
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: adde26095a11016796f6bd616978c3ccc0fe4c18
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618845"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="a151b-103">Возможности, добавленные в версии EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="a151b-103">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="a151b-104">Платформы</span><span class="sxs-lookup"><span data-stu-id="a151b-104">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="a151b-105">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="a151b-105">.NET Framework 4.5.1</span></span>

<span data-ttu-id="a151b-106">Включает консоль, WPF, WinForms, ASP.NET 4 и многое другое.</span><span class="sxs-lookup"><span data-stu-id="a151b-106">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="a151b-107">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="a151b-107">.NET Standard 1.3</span></span>

<span data-ttu-id="a151b-108">Поддержка ASP.NET Core для .NET Framework и .NET Core в операционных системах Windows, OSX и Linux.</span><span class="sxs-lookup"><span data-stu-id="a151b-108">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="a151b-109">Моделирование</span><span class="sxs-lookup"><span data-stu-id="a151b-109">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="a151b-110">Базовое моделирование</span><span class="sxs-lookup"><span data-stu-id="a151b-110">Basic modelling</span></span>

<span data-ttu-id="a151b-111">На основе сущностей POCO со свойствами get и set для стандартных скалярных типов (`int`, `string` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="a151b-111">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="a151b-112">Связи и свойства навигации</span><span class="sxs-lookup"><span data-stu-id="a151b-112">Relationships and navigation properties</span></span>

<span data-ttu-id="a151b-113">В модели на основе внешнего ключа можно создавать отношения "один ко многим" и "один к не более чем одному".</span><span class="sxs-lookup"><span data-stu-id="a151b-113">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="a151b-114">С этими отношениями можно связать свойства навигации для простой коллекции или ссылочных типов.</span><span class="sxs-lookup"><span data-stu-id="a151b-114">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="a151b-115">Встроенные соглашения</span><span class="sxs-lookup"><span data-stu-id="a151b-115">Built-in conventions</span></span>

<span data-ttu-id="a151b-116">В этом варианте исходная модель создается на основе формы классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="a151b-116">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="a151b-117">Текучий API</span><span class="sxs-lookup"><span data-stu-id="a151b-117">Fluent API</span></span>

<span data-ttu-id="a151b-118">Позволяет переопределять метод `OnModelCreating` для пользовательского контекста, чтобы настраивать модель, обнаруженную на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="a151b-118">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="a151b-119">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="a151b-119">Data annotations</span></span>

<span data-ttu-id="a151b-120">Это атрибуты, которые можно добавлять в классы и (или) свойства сущности и которые будут влиять на модели EF.</span><span class="sxs-lookup"><span data-stu-id="a151b-120">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="a151b-121">Например, добавление `[Required]` сообщит платформе EF, что соответствующее свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="a151b-121">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="a151b-122">Сопоставление в реляционной таблице</span><span class="sxs-lookup"><span data-stu-id="a151b-122">Relational Table mapping</span></span>

<span data-ttu-id="a151b-123">Позволяет сопоставить сущности с таблицами и (или) столбцами.</span><span class="sxs-lookup"><span data-stu-id="a151b-123">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="a151b-124">Создание значений ключа</span><span class="sxs-lookup"><span data-stu-id="a151b-124">Key value generation</span></span>

<span data-ttu-id="a151b-125">Поддерживается создание на стороне клиентских процессов или в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-125">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="a151b-126">Созданные базой данных значения</span><span class="sxs-lookup"><span data-stu-id="a151b-126">Database generated values</span></span>

<span data-ttu-id="a151b-127">Позволяет базе данных создавать значения при вставке (для значений по умолчанию) или обновлении данных (для вычисляемых столбцов).</span><span class="sxs-lookup"><span data-stu-id="a151b-127">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="a151b-128">Последовательности в SQL Server</span><span class="sxs-lookup"><span data-stu-id="a151b-128">Sequences in SQL Server</span></span>

<span data-ttu-id="a151b-129">Позволяет определить в модели объекты последовательностей.</span><span class="sxs-lookup"><span data-stu-id="a151b-129">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="a151b-130">Ограничения уникальности</span><span class="sxs-lookup"><span data-stu-id="a151b-130">Unique constraints</span></span>

<span data-ttu-id="a151b-131">Позволяет определить альтернативные ключи и связи, использующие такие ключи.</span><span class="sxs-lookup"><span data-stu-id="a151b-131">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="a151b-132">Индексы</span><span class="sxs-lookup"><span data-stu-id="a151b-132">Indexes</span></span>

<span data-ttu-id="a151b-133">При определении индексов в модели они автоматически добавляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-133">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="a151b-134">Поддерживаются также индексы UNIQUE.</span><span class="sxs-lookup"><span data-stu-id="a151b-134">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="a151b-135">Теневые свойства состояния</span><span class="sxs-lookup"><span data-stu-id="a151b-135">Shadow state properties</span></span>

<span data-ttu-id="a151b-136">Возможность определить в модели свойства, которые не объявляются и не хранятся в классе .NET, но отслеживаются и обновляются с помощью EF Core.</span><span class="sxs-lookup"><span data-stu-id="a151b-136">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="a151b-137">Чаще всего применяется для свойств внешнего ключа, которые нежелательно отображать в самом объекте.</span><span class="sxs-lookup"><span data-stu-id="a151b-137">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="a151b-138">Шаблон наследования "одна таблица на иерархию"</span><span class="sxs-lookup"><span data-stu-id="a151b-138">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="a151b-139">Позволяет сохранять несколько сущностей иерархии в одну таблицу, в которой столбец дискриминатора определяет тип сущности для каждой записи в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-139">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="a151b-140">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="a151b-140">Model validation</span></span>

<span data-ttu-id="a151b-141">Обнаруживает в модели неправильные соотношения и предоставляет полезные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="a151b-141">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="a151b-142">отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="a151b-142">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="a151b-143">Отслеживание изменений по моментальному снимку</span><span class="sxs-lookup"><span data-stu-id="a151b-143">Snapshot change tracking</span></span>

<span data-ttu-id="a151b-144">Позволяет автоматически обнаруживать изменения в сущностях, сравнивая их текущее состояние с копией (моментальным снимком) исходного состояния.</span><span class="sxs-lookup"><span data-stu-id="a151b-144">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="a151b-145">Извещения об изменении состояния</span><span class="sxs-lookup"><span data-stu-id="a151b-145">Notification change tracking</span></span>

<span data-ttu-id="a151b-146">Позволяет создавать уведомления для отслеживающей стороны при изменении значений свойств.</span><span class="sxs-lookup"><span data-stu-id="a151b-146">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="a151b-147">Доступ к отслеживаемому состоянию</span><span class="sxs-lookup"><span data-stu-id="a151b-147">Accessing tracked state</span></span>

<span data-ttu-id="a151b-148">Осуществляется через `DbContext.Entry` и `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="a151b-148">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="a151b-149">Присоединение отсоединенных сущностей и диаграмм</span><span class="sxs-lookup"><span data-stu-id="a151b-149">Attaching detached entities/graphs</span></span>

<span data-ttu-id="a151b-150">Новый API `DbContext.AttachGraph` позволяет повторно присоединить сущности к контексту, чтобы избежать лишних операций создания или изменения объектов.</span><span class="sxs-lookup"><span data-stu-id="a151b-150">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="a151b-151">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="a151b-151">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="a151b-152">Основные возможности функции сохранения</span><span class="sxs-lookup"><span data-stu-id="a151b-152">Basic save functionality</span></span>

<span data-ttu-id="a151b-153">Изменения экземпляров сущности могут сохраняться в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-153">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="a151b-154">Оптимистическая блокировка</span><span class="sxs-lookup"><span data-stu-id="a151b-154">Optimistic Concurrency</span></span>

<span data-ttu-id="a151b-155">Защита от перезаписи изменений, внесенных другим пользователем после получения данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-155">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="a151b-156">Асинхронный метод сохранения SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a151b-156">Async SaveChanges</span></span>

<span data-ttu-id="a151b-157">Позволяет освободить текущий поток для обработки других запросов, пока база данных обрабатывает команды от `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a151b-157">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="a151b-158">Транзакции базы данных</span><span class="sxs-lookup"><span data-stu-id="a151b-158">Database Transactions</span></span>

<span data-ttu-id="a151b-159">Означает, что `SaveChanges` всегда является атомарной (то есть либо завершается успешно полностью, либо не вносит никаких изменений в базу данных).</span><span class="sxs-lookup"><span data-stu-id="a151b-159">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="a151b-160">Также для транзакций предоставлены специальные API, которые позволяют совместно использовать транзакции в нескольких экземплярах контекста и т. д.</span><span class="sxs-lookup"><span data-stu-id="a151b-160">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="a151b-161">Реляционные базы данных: пакетная обработка инструкций</span><span class="sxs-lookup"><span data-stu-id="a151b-161">Relational: Batching of statements</span></span>

<span data-ttu-id="a151b-162">Повышает производительность, выполняя несколько команд INSERT/UPDATE/DELETE за одно обращение к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-162">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="a151b-163">Запрос</span><span class="sxs-lookup"><span data-stu-id="a151b-163">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="a151b-164">Базовая поддержка LINQ</span><span class="sxs-lookup"><span data-stu-id="a151b-164">Basic LINQ support</span></span>

<span data-ttu-id="a151b-165">Предоставляет возможность использовать LINQ для извлечения данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-165">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="a151b-166">Смешанное вычисление на клиенте и сервере</span><span class="sxs-lookup"><span data-stu-id="a151b-166">Mixed client/server evaluation</span></span>

<span data-ttu-id="a151b-167">Позволяет включать в запросы логику, которая не может быть выполнена в базе данных, и обрабатывать ее уже после получения данных в память.</span><span class="sxs-lookup"><span data-stu-id="a151b-167">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="a151b-168">NoTracking</span><span class="sxs-lookup"><span data-stu-id="a151b-168">NoTracking</span></span>

<span data-ttu-id="a151b-169">Запросы выполняются быстрее, если контекст не отслеживает изменения экземпляров сущностей (это полезно, если результаты доступны только для чтения).</span><span class="sxs-lookup"><span data-stu-id="a151b-169">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="a151b-170">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="a151b-170">Eager loading</span></span>

<span data-ttu-id="a151b-171">Предоставляет методы `Include` и `ThenInclude` для определения смежных данных, которые нужно извлекать вместе с основным запросом.</span><span class="sxs-lookup"><span data-stu-id="a151b-171">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="a151b-172">Асинхронный запрос</span><span class="sxs-lookup"><span data-stu-id="a151b-172">Async query</span></span>

<span data-ttu-id="a151b-173">Позволяет освободить текущий поток и его ресурсы для обработки других запросов, пока база данных обрабатывает текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="a151b-173">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="a151b-174">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="a151b-174">Raw SQL queries</span></span>

<span data-ttu-id="a151b-175">Предоставляет метод `DbSet.FromSql` для получения данных с помощью необработанных SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="a151b-175">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="a151b-176">Эти запросы можно также создавать с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="a151b-176">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="a151b-177">Управление схемой базы данных</span><span class="sxs-lookup"><span data-stu-id="a151b-177">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="a151b-178">Интерфейсы API для создания и удаления баз данных</span><span class="sxs-lookup"><span data-stu-id="a151b-178">Database creation/deletion APIs</span></span>

<span data-ttu-id="a151b-179">Предназначены в основном для тестирования, при котором часто бывает нужно быстро создать или удалить базу данных без использования миграции.</span><span class="sxs-lookup"><span data-stu-id="a151b-179">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="a151b-180">Миграции реляционной базы данных</span><span class="sxs-lookup"><span data-stu-id="a151b-180">Relational database migrations</span></span>

<span data-ttu-id="a151b-181">Позволяет изменять схему реляционной базы данных параллельно с развитием модели.</span><span class="sxs-lookup"><span data-stu-id="a151b-181">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="a151b-182">Реконструирование из базы данных</span><span class="sxs-lookup"><span data-stu-id="a151b-182">Reverse engineer from database</span></span>

<span data-ttu-id="a151b-183">Создает модель EF на основе существующей схемы реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-183">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="a151b-184">Поставщики баз данных</span><span class="sxs-lookup"><span data-stu-id="a151b-184">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="a151b-185">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a151b-185">SQL Server</span></span>

<span data-ttu-id="a151b-186">Позволяет подключаться к Microsoft SQL Server 2008 и более новых версий.</span><span class="sxs-lookup"><span data-stu-id="a151b-186">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="a151b-187">SQLite</span><span class="sxs-lookup"><span data-stu-id="a151b-187">SQLite</span></span>

<span data-ttu-id="a151b-188">Позволяет подключаться к базе данных SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="a151b-188">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="a151b-189">В памяти</span><span class="sxs-lookup"><span data-stu-id="a151b-189">In-Memory</span></span>

<span data-ttu-id="a151b-190">Позволяет легко выполнять тестирование без подключения к настоящим базам данных.</span><span class="sxs-lookup"><span data-stu-id="a151b-190">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="a151b-191">Сторонние поставщики</span><span class="sxs-lookup"><span data-stu-id="a151b-191">3rd party providers</span></span>

<span data-ttu-id="a151b-192">Поддерживаются несколько поставщиков других ядер СУБД.</span><span class="sxs-lookup"><span data-stu-id="a151b-192">Several providers are available for other database engines.</span></span> <span data-ttu-id="a151b-193">Полный их список можно найти в статье [Database Providers](xref:core/providers/index) (Поставщики баз данных).</span><span class="sxs-lookup"><span data-stu-id="a151b-193">See [Database Providers](xref:core/providers/index) for a complete list.</span></span>
