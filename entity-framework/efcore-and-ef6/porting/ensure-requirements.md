---
title: Перенос приложений из EF6 в EF Core — Проверка требований
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993444"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="9e5b7-102">Прежде чем перенос приложений из EF6 в EF Core: проверка требования вашего приложения</span><span class="sxs-lookup"><span data-stu-id="9e5b7-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="9e5b7-103">Перед началом процесса переноса важно проверить EF Core, соответствует ли требований доступа к данным для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="9e5b7-104">Для отсутствующих компонентов</span><span class="sxs-lookup"><span data-stu-id="9e5b7-104">Missing features</span></span>

<span data-ttu-id="9e5b7-105">Убедитесь, что EF Core имеет все функции, которые необходимо использовать в приложении.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="9e5b7-106">См. в разделе [сравнение функций](../features.md) подробное сравнение сравнение с набором функций из EF Core с EF6.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="9e5b7-107">Если все необходимые компоненты отсутствуют, убедитесь, что вы можете компенсировать отсутствие этих функций перед развертыванием в EF Core.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="9e5b7-108">Изменения в поведении</span><span class="sxs-lookup"><span data-stu-id="9e5b7-108">Behavior changes</span></span>

<span data-ttu-id="9e5b7-109">Это неполный список некоторые изменения в поведении между EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="9e5b7-110">Важно помнить как порт приложения, так как они могут изменить способ приложение ведет себя, но не будут отображаться как ошибки компиляции после переключения в EF Core.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="9e5b7-111">Поведение DbSet.Add/Attach и graph</span><span class="sxs-lookup"><span data-stu-id="9e5b7-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="9e5b7-112">В EF6 вызвав `DbSet.Add()` на результаты сущностей в рекурсивный поиск для всех сущностей, на которые ссылается его свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="9e5b7-113">Все сущности, на которые имеются и, не отслеживаются контекстом, можно пометить как добавлен.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="9e5b7-114">`DbSet.Attach()` работает так же, за исключением того, все сущности, помечаются как без изменений.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="9e5b7-115">**EF Core выполняет рекурсивный поиск аналогичный, но в некоторых правил немного отличается.**</span><span class="sxs-lookup"><span data-stu-id="9e5b7-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="9e5b7-116">Корневая сущность всегда указывается в запрошенное состояние (для `DbSet.Add` и не изменяется для `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="9e5b7-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="9e5b7-117">**Для сущностей, которые были обнаружены во время рекурсивный поиск свойств навигации:**</span><span class="sxs-lookup"><span data-stu-id="9e5b7-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="9e5b7-118">**Если первичный ключ сущности сохраняются созданные**</span><span class="sxs-lookup"><span data-stu-id="9e5b7-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="9e5b7-119">Если первичный ключ не присвоено значение, состояние устанавливается на добавлена.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="9e5b7-120">Значение первичного ключа считается «не установлено» назначена CLR по умолчанию для типа свойства (например, `0` для `int`, `null` для `string`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="9e5b7-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="9e5b7-121">Если первичный ключ, присваивается значение, состояние имеет значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="9e5b7-122">Если первичный ключ не сформированный базой данных, сущность помещается в том же состоянии, в качестве привилегированного пользователя.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="9e5b7-123">Код инициализации первой базы данных</span><span class="sxs-lookup"><span data-stu-id="9e5b7-123">Code First database initialization</span></span>

<span data-ttu-id="9e5b7-124">**EF6 имеет значительный объем magic, выполняемых по выбору подключения к базе данных и инициализации базы данных. Ниже перечислены некоторые из этих правил.**</span><span class="sxs-lookup"><span data-stu-id="9e5b7-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="9e5b7-125">Если конфигурация не выполняется, EF6 выберет базу данных в SQL Express или LocalDb.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="9e5b7-126">Если строка подключения с тем же именем, как контекст — в приложениях `App/Web.config` файл, будет использоваться это соединение.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="9e5b7-127">Если базы данных не существует, он создается.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="9e5b7-128">Если ни одной из таблиц из модели существует в базе данных, схема для текущей модели добавляется в базу данных.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="9e5b7-129">Если включены миграций, они используются для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="9e5b7-130">Если база данных существует и EF6 ранее создали схемы, схемы проверяется на совместимость с текущей моделью.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="9e5b7-131">Исключение возникает в том случае, если модель данных была изменена с момента создания схемы.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="9e5b7-132">**EF Core не выполняет эти невероятные вещи.**</span><span class="sxs-lookup"><span data-stu-id="9e5b7-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="9e5b7-133">Подключение к базе данных должна быть настроена явным образом в коде.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="9e5b7-134">Инициализация не выполняется.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-134">No initialization is performed.</span></span> <span data-ttu-id="9e5b7-135">Необходимо использовать `DbContext.Database.Migrate()` по применению миграции (или `DbContext.Database.EnsureCreated()` и `EnsureDeleted()` для создания и удаления базы данных без использования миграции).</span><span class="sxs-lookup"><span data-stu-id="9e5b7-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="9e5b7-136">Код первой таблицы, соглашение об именовании</span><span class="sxs-lookup"><span data-stu-id="9e5b7-136">Code First table naming convention</span></span>

<span data-ttu-id="9e5b7-137">EF6 выполняется с именем класса сущностей через службу преобразования во множественную форму, для которого требуется вычислить имя таблицы по умолчанию, который сопоставляется сущность.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="9e5b7-138">EF Core использует имя `DbSet` свойства, предоставляемого сущность в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="9e5b7-139">Если сущность не содержит `DbSet` используется свойство, а затем имя класса.</span><span class="sxs-lookup"><span data-stu-id="9e5b7-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
