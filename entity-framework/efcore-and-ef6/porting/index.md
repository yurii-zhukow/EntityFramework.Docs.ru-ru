---
title: Перенос приложений из EF6 в EF Core — EF
description: Общие сведения о переносе приложения с Entity Framework 6 в Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: d50def47e65455c8cf5242cad4386f157148c0bc
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429212"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="27298-103">Перенос приложений из EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="27298-103">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="27298-104">Из-за фундаментальных отличий от EF Core мы не рекомендуем переносить приложения EF6 на EF Core, если для этого нет веской причины.</span><span class="sxs-lookup"><span data-stu-id="27298-104">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="27298-105">Переход с EF6 на EF Core следует рассматривать как портирование, а не обновление.</span><span class="sxs-lookup"><span data-stu-id="27298-105">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="27298-106">Прежде чем начать перенос, следует убедиться, что EF Core соответствует требованиям приложения к доступу к данным.</span><span class="sxs-lookup"><span data-stu-id="27298-106">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="27298-107">Отсутствующие функции</span><span class="sxs-lookup"><span data-stu-id="27298-107">Missing features</span></span>

<span data-ttu-id="27298-108">Убедитесь, что EF Core обладает всеми функциями, которые нужны для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="27298-108">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="27298-109">Ознакомьтесь с [подробным сравнением набора функций](xref:efcore-and-ef6/index) EF Core и EF6.</span><span class="sxs-lookup"><span data-stu-id="27298-109">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="27298-110">Если отсутствуют необходимые функции, перед выполнением переноса в EF Core убедитесь, что вы можете компенсировать их отсутствие.</span><span class="sxs-lookup"><span data-stu-id="27298-110">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="27298-111">Изменения в работе</span><span class="sxs-lookup"><span data-stu-id="27298-111">Behavior changes</span></span>

<span data-ttu-id="27298-112">Это не исчерпывающий список некоторых изменений в поведении между EF6 и EF Core.</span><span class="sxs-lookup"><span data-stu-id="27298-112">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="27298-113">Эти факторы важно учитывать при переносе своего приложения, так как его поведение может измениться. После перехода на EF Core такие изменения не будут отображаться как ошибки компиляции.</span><span class="sxs-lookup"><span data-stu-id="27298-113">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="27298-114">Поведение методов DbSet.Add и DbSet.Attach и графа</span><span class="sxs-lookup"><span data-stu-id="27298-114">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="27298-115">В EF6 при вызове метода `DbSet.Add()` для сущности происходит рекурсивный поиск по всем сущностям, на которые указаны ссылки в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="27298-115">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="27298-116">Все найденные сущности, которые еще не отслеживаются контекстом, также помечаются как добавленные.</span><span class="sxs-lookup"><span data-stu-id="27298-116">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="27298-117">`DbSet.Attach()` работает так же, за исключением того, что все сущности помечаются как неизмененные.</span><span class="sxs-lookup"><span data-stu-id="27298-117">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="27298-118">**EF Core выполняет аналогичный рекурсивный поиск, но с несколько иными правилами.**</span><span class="sxs-lookup"><span data-stu-id="27298-118">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

* <span data-ttu-id="27298-119">Корневая сущность всегда находится в запрошенном состоянии (added (Добавлено) для `DbSet.Add` и unchanged (Не изменено) для `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="27298-119">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>
* <span data-ttu-id="27298-120">**Для сущностей, найденных во время рекурсивного поиска свойств навигации:**</span><span class="sxs-lookup"><span data-stu-id="27298-120">**For entities that are found during the recursive search of navigation properties:**</span></span>
  * <span data-ttu-id="27298-121">**Если первичный ключ сущности создан хранилищем**</span><span class="sxs-lookup"><span data-stu-id="27298-121">**If the primary key of the entity is store generated**</span></span>
    * <span data-ttu-id="27298-122">Если первичному ключу не присвоено значение, то устанавливается состояние added.</span><span class="sxs-lookup"><span data-stu-id="27298-122">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="27298-123">Значение первичного ключа считается не присвоенным, если для типа свойства задано значение CLR по умолчанию (например, `0` для `int`, `null` для `string` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="27298-123">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>
    * <span data-ttu-id="27298-124">Если первичному ключу присвоено значение, то устанавливается состояние unchanged.</span><span class="sxs-lookup"><span data-stu-id="27298-124">If the primary key is set to a value, the state is set to unchanged.</span></span>
  * <span data-ttu-id="27298-125">Если первичный ключ не создан базой данных, то сущность помещается в то же состояние, что и корневая сущность.</span><span class="sxs-lookup"><span data-stu-id="27298-125">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="27298-126">Инициализация базы данных с использованием Code First</span><span class="sxs-lookup"><span data-stu-id="27298-126">Code First database initialization</span></span>

<span data-ttu-id="27298-127">**В EF6 присутствует множество сложностей при выборе подключения к базе данных и ее инициализации. Вот некоторые из таких правил:**</span><span class="sxs-lookup"><span data-stu-id="27298-127">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="27298-128">Если конфигурация не выполняется, EF6 выберет базу данных в SQL Express или LocalDb.</span><span class="sxs-lookup"><span data-stu-id="27298-128">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="27298-129">Если в файле приложения `App/Web.config` присутствует строка подключения с таким же именем, как у контекста, подключение будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="27298-129">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="27298-130">Если база данных не существует, она будет создана.</span><span class="sxs-lookup"><span data-stu-id="27298-130">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="27298-131">Если ни одной из таблиц модели нет в базе данных, то в базу данных добавляется схема для текущей модели.</span><span class="sxs-lookup"><span data-stu-id="27298-131">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="27298-132">Если включены миграции, то они используются для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="27298-132">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="27298-133">Если база данных существует и в EF6 схема была создана ранее, то она проверяется на совместимость с текущей моделью.</span><span class="sxs-lookup"><span data-stu-id="27298-133">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="27298-134">Если же модель изменилась с момента создания схемы, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="27298-134">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="27298-135">**В EF Core нет таких сложных операций.**</span><span class="sxs-lookup"><span data-stu-id="27298-135">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="27298-136">Подключение к базе данных должно быть явно настроено в коде.</span><span class="sxs-lookup"><span data-stu-id="27298-136">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="27298-137">Инициализация не выполняется.</span><span class="sxs-lookup"><span data-stu-id="27298-137">No initialization is performed.</span></span> <span data-ttu-id="27298-138">Для применения миграций нужно использовать `DbContext.Database.Migrate()`. Либо используйте `DbContext.Database.EnsureCreated()` и `EnsureDeleted()` для создания или удаления базы данных без применения миграций.</span><span class="sxs-lookup"><span data-stu-id="27298-138">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="27298-139">Соглашение об именовании таблиц с использованием Code First</span><span class="sxs-lookup"><span data-stu-id="27298-139">Code First table naming convention</span></span>

<span data-ttu-id="27298-140">EF6 передает имя класса сущности в службу преобразования во множественную форму, чтобы вычислить имя таблицы по умолчанию, с которой сопоставлена сущность.</span><span class="sxs-lookup"><span data-stu-id="27298-140">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="27298-141">В EF Core используется имя свойства `DbSet`, в котором предоставлена сущность в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="27298-141">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="27298-142">Если для сущности нет свойства `DbSet`, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="27298-142">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
