---
title: Перенос с EF6 на EF Core — проверка требований
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565345"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="1ce09-102">Перед переносом из EF6 в EF Core: Проверка требований приложения</span><span class="sxs-lookup"><span data-stu-id="1ce09-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="1ce09-103">Перед началом процесса переноса важно проверить, что EF Core соответствует требованиям к доступу к данным приложения.</span><span class="sxs-lookup"><span data-stu-id="1ce09-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="1ce09-104">Отсутствующие компоненты</span><span class="sxs-lookup"><span data-stu-id="1ce09-104">Missing features</span></span>

<span data-ttu-id="1ce09-105">Убедитесь, что EF Core имеет все функции, необходимые для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="1ce09-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="1ce09-106">Подробное сравнение того, как набор функций в EF Core сравнивается с EF6, см. в разделе [Сравнение характеристик](../features.md) .</span><span class="sxs-lookup"><span data-stu-id="1ce09-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="1ce09-107">Если отсутствуют необходимые компоненты, перед переносом в EF Core убедитесь, что вы можете компенсировать отсутствие этих функций.</span><span class="sxs-lookup"><span data-stu-id="1ce09-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="1ce09-108">Изменения в поведении</span><span class="sxs-lookup"><span data-stu-id="1ce09-108">Behavior changes</span></span>

<span data-ttu-id="1ce09-109">Это не исчерпывающий список некоторых изменений в поведении между EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ce09-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="1ce09-110">Важно помнить о том, что ваш порт приложения может измениться в том виде, в каком он работает в приложении, но не будет отображаться как ошибки компиляции после переключения на EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ce09-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="1ce09-111">Поведение DbSet. Add/Attach и Graph</span><span class="sxs-lookup"><span data-stu-id="1ce09-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="1ce09-112">В EF6 вызов `DbSet.Add()` сущности приводит к рекурсивному поиску всех сущностей, на которые имеются ссылки в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="1ce09-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="1ce09-113">Все сущности, которые обнаружены и еще не отправляются контекстом, также помечаются как добавленные.</span><span class="sxs-lookup"><span data-stu-id="1ce09-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="1ce09-114">`DbSet.Attach()`ведет себя одинаково, за исключением того, что все сущности помечаются как неизмененные.</span><span class="sxs-lookup"><span data-stu-id="1ce09-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="1ce09-115">**EF Core выполняет аналогичный рекурсивный поиск, но с некоторыми разными правилами.**</span><span class="sxs-lookup"><span data-stu-id="1ce09-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="1ce09-116">Корневая сущность всегда находится в запрошенном состоянии (добавляется `DbSet.Add` для и не изменяется для `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="1ce09-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="1ce09-117">**Для сущностей, найденных во время рекурсивного поиска свойств навигации:**</span><span class="sxs-lookup"><span data-stu-id="1ce09-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="1ce09-118">**Если первичный ключ сущности — хранилище создано**</span><span class="sxs-lookup"><span data-stu-id="1ce09-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="1ce09-119">Если первичному ключу не присвоено значение, то для него устанавливается состояние добавлено.</span><span class="sxs-lookup"><span data-stu-id="1ce09-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="1ce09-120">Значение первичного ключа считается "не задано", если ему присвоено значение `0` CLR по умолчанию для типа свойства (например, `int` `null` для, `string`для и т. д.).</span><span class="sxs-lookup"><span data-stu-id="1ce09-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="1ce09-121">Если для первичного ключа задано значение, то для него устанавливается состояние без изменений.</span><span class="sxs-lookup"><span data-stu-id="1ce09-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="1ce09-122">Если первичный ключ не создается базой данных, то сущность помещается в то же состояние, что и корень.</span><span class="sxs-lookup"><span data-stu-id="1ce09-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="1ce09-123">Инициализация базы данных Code First</span><span class="sxs-lookup"><span data-stu-id="1ce09-123">Code First database initialization</span></span>

<span data-ttu-id="1ce09-124">**EF6 обладает значительным количеством волшебных ИТ-проблем, связанных с выбором подключения к базе данных и инициализацией базы данных. Ниже перечислены некоторые из этих правил.**</span><span class="sxs-lookup"><span data-stu-id="1ce09-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="1ce09-125">Если конфигурация не выполняется, EF6 выберет базу данных в SQL Express или LocalDb.</span><span class="sxs-lookup"><span data-stu-id="1ce09-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="1ce09-126">Если строка подключения с тем же именем, что и у контекста, находится в `App/Web.config` файле приложения, будет использоваться это соединение.</span><span class="sxs-lookup"><span data-stu-id="1ce09-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="1ce09-127">Если база данных не существует, она будет создана.</span><span class="sxs-lookup"><span data-stu-id="1ce09-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="1ce09-128">Если ни одна из таблиц модели не существует в базе данных, то схема для текущей модели добавляется в базу данных.</span><span class="sxs-lookup"><span data-stu-id="1ce09-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="1ce09-129">Если миграция включена, они используются для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="1ce09-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="1ce09-130">Если база данных существует и EF6 ранее создала схему, то схема проверяется на совместимость с текущей моделью.</span><span class="sxs-lookup"><span data-stu-id="1ce09-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="1ce09-131">Исключение возникает, если модель изменилась с момента создания схемы.</span><span class="sxs-lookup"><span data-stu-id="1ce09-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="1ce09-132">**EF Core не выполняет никаких этих волшебных работ.**</span><span class="sxs-lookup"><span data-stu-id="1ce09-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="1ce09-133">Подключение к базе данных должно быть явно настроено в коде.</span><span class="sxs-lookup"><span data-stu-id="1ce09-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="1ce09-134">Инициализация не выполняется.</span><span class="sxs-lookup"><span data-stu-id="1ce09-134">No initialization is performed.</span></span> <span data-ttu-id="1ce09-135">Для применения миграции `DbContext.Database.Migrate()` ( `DbContext.Database.EnsureCreated()` `EnsureDeleted()` или для создания или удаления базы данных без использования миграций) необходимо использовать.</span><span class="sxs-lookup"><span data-stu-id="1ce09-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="1ce09-136">Соглашение об именовании таблиц Code First</span><span class="sxs-lookup"><span data-stu-id="1ce09-136">Code First table naming convention</span></span>

<span data-ttu-id="1ce09-137">EF6 запускает имя класса сущности через службу множественного преобразования, чтобы вычислить имя таблицы по умолчанию, с которой сопоставлена сущность.</span><span class="sxs-lookup"><span data-stu-id="1ce09-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="1ce09-138">EF Core использует имя `DbSet` свойства, которое предоставляется сущностью в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="1ce09-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="1ce09-139">Если сущность не имеет `DbSet` свойства, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="1ce09-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
