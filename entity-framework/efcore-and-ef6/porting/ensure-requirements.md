---
title: Перенос приложений из EF6 в основные EF – Проверка требований
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052864"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="3cf2d-102">Перед развертыванием с EF6 EF ядра: проверка требования вашего приложения</span><span class="sxs-lookup"><span data-stu-id="3cf2d-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="3cf2d-103">Перед началом процедуры перевода важно проверить соответствие EF основных требований доступа к данным для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="3cf2d-104">Для отсутствующих компонентов</span><span class="sxs-lookup"><span data-stu-id="3cf2d-104">Missing features</span></span>

<span data-ttu-id="3cf2d-105">Убедитесь, что EF Core все компоненты, необходимые для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="3cf2d-106">В разделе [сравнение функций](../features.md) для выполнения в ядре EF набор функций, в сравнении с EF6 подробное сравнение.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="3cf2d-107">Если все необходимые компоненты отсутствуют, убедитесь, вы можете компенсировать отсутствие этих функций перед развертыванием EF ядра.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="3cf2d-108">Изменения в поведении</span><span class="sxs-lookup"><span data-stu-id="3cf2d-108">Behavior changes</span></span>

<span data-ttu-id="3cf2d-109">Это неполном списке внесены изменения в поведении между EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="3cf2d-110">Важно сохранить эти учитывать как порт приложения, они могут изменять способ приложение ведет себя, но не будут отображаться как ошибки компиляции после замены EF ядра.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="3cf2d-111">Поведение DbSet.Add/Attach и диаграммы</span><span class="sxs-lookup"><span data-stu-id="3cf2d-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="3cf2d-112">В EF6 вызвав `DbSet.Add()` в сущности приводит к рекурсивный поиск для всех сущностей, на которые ссылается его свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="3cf2d-113">Все сущности, которые находятся, а не уже отслеживается контекстом, которые также будут отмечены как добавлен.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="3cf2d-114">`DbSet.Attach()`ведет себя так же, за исключением того, все сущности, помечаются как без изменений.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="3cf2d-115">**EF Core выполняет аналогичные рекурсивный поиск, но с некоторыми немного другие правила.**</span><span class="sxs-lookup"><span data-stu-id="3cf2d-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="3cf2d-116">Корневой объект всегда находится в запрошенное состояние (добавлен для `DbSet.Add` и без изменений для `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="3cf2d-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="3cf2d-117">**Для сущностей, которые были обнаружены во время рекурсивный поиск свойств навигации:**</span><span class="sxs-lookup"><span data-stu-id="3cf2d-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="3cf2d-118">**Если первичный ключ сущности, формируемый хранилищем**</span><span class="sxs-lookup"><span data-stu-id="3cf2d-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="3cf2d-119">Если первичный ключ не присвоено значение, переходит в состояние до добавлены.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="3cf2d-120">Значение первичного ключа считается «не установлено», если ей назначается значение по умолчанию для типа свойства CLR (т. е. `0` для `int`, `null` для `string`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="3cf2d-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="3cf2d-121">Если первичный ключ присвоено значение, состояние имеет значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="3cf2d-122">Если первичный ключ не создается базой данных, сущность будет помещен в том же состоянии, как корень.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="3cf2d-123">Код инициализации первой базы данных</span><span class="sxs-lookup"><span data-stu-id="3cf2d-123">Code First database initialization</span></span>

<span data-ttu-id="3cf2d-124">**EF6 имеет значительный объем magic, выполняемых по выбору подключения к базе данных и инициализации базы данных. Ниже перечислены некоторые из этих правил.**</span><span class="sxs-lookup"><span data-stu-id="3cf2d-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="3cf2d-125">Если конфигурация не выполняется, EF6 выберет базу данных на SQL Express или LocalDb.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="3cf2d-126">Если строка подключения с тем же именем, как контекст в приложениях `App/Web.config` файла, будет использоваться это соединение.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="3cf2d-127">Если базы данных не существует, он создается.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="3cf2d-128">Если ни одна из таблиц из модели существуют в базе данных, схемы для текущей модели добавляется в базу данных.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="3cf2d-129">Если миграция включена, они используются для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="3cf2d-130">Если база данных существует и EF6 был создан ранее схемы, схемы проверяется на совместимость с текущей моделью.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="3cf2d-131">Если модель данных была изменена с момента создания схемы исключение.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="3cf2d-132">**Основные EF не выполняет это магическое значение.**</span><span class="sxs-lookup"><span data-stu-id="3cf2d-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="3cf2d-133">Подключение к базе данных должен быть явно настроен в коде.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="3cf2d-134">То инициализация не выполняется.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-134">No initialization is performed.</span></span> <span data-ttu-id="3cf2d-135">Необходимо использовать `DbContext.Database.Migrate()` для применения миграций (или `DbContext.Database.EnsureCreated()` и `EnsureDeleted()` для создания и удаления базы данных без использования миграции).</span><span class="sxs-lookup"><span data-stu-id="3cf2d-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="3cf2d-136">Код соглашения об именах для первой таблицы</span><span class="sxs-lookup"><span data-stu-id="3cf2d-136">Code First table naming convention</span></span>

<span data-ttu-id="3cf2d-137">EF6 завершит службы преобразования во множественную форму для вычисления имя таблицы по умолчанию, который сопоставляется сущность с именем класса сущностей.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="3cf2d-138">EF Core использует имя `DbSet` свойства, предоставляемую сущности в производном контекста.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="3cf2d-139">Если сущность не имеет `DbSet` используется свойство, а затем имени класса.</span><span class="sxs-lookup"><span data-stu-id="3cf2d-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
