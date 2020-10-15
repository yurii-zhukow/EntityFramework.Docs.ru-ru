---
title: Типы сущностей — EF Core
description: Как настроить и сопоставлять типы сущностей с помощью Entity Framework Core
author: roji
ms.date: 10/06/2020
uid: core/modeling/entity-types
ms.openlocfilehash: bfefa29c08679a1524c00769b3495d75a301e2d3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062234"
---
# <a name="entity-types"></a><span data-ttu-id="e9f06-103">Типы сущностей</span><span class="sxs-lookup"><span data-stu-id="e9f06-103">Entity Types</span></span>

<span data-ttu-id="e9f06-104">Включение DbSet типа в контекст означает, что он включен в модель EF Core; как правило, тип сущности упоминается как *сущность*.</span><span class="sxs-lookup"><span data-stu-id="e9f06-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="e9f06-105">EF Core может считывать и записывать экземпляры сущностей из базы данных, а также, если используется реляционная база данных, EF Core может создавать таблицы для сущностей с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="e9f06-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="e9f06-106">Включение типов в модель</span><span class="sxs-lookup"><span data-stu-id="e9f06-106">Including types in the model</span></span>

<span data-ttu-id="e9f06-107">По соглашению типы, предоставляемые в свойствах DbSet в контексте, включаются в модель в качестве сущностей.</span><span class="sxs-lookup"><span data-stu-id="e9f06-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="e9f06-108">Также включаются типы сущностей, указанные в `OnModelCreating` методе, как и любые типы, которые обнаруживаются рекурсивным просмотром свойств навигации других обнаруженных типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="e9f06-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="e9f06-109">В приведенном ниже примере кода включены все типы:</span><span class="sxs-lookup"><span data-stu-id="e9f06-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="e9f06-110">`Blog` включен, так как он предоставляется в свойстве DbSet в контексте.</span><span class="sxs-lookup"><span data-stu-id="e9f06-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="e9f06-111">`Post` включен, так как он обнаруживается через `Blog.Posts` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="e9f06-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="e9f06-112">`AuditEntry` так как он указан в `OnModelCreating` .</span><span class="sxs-lookup"><span data-stu-id="e9f06-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="e9f06-113">Исключение типов из модели</span><span class="sxs-lookup"><span data-stu-id="e9f06-113">Excluding types from the model</span></span>

<span data-ttu-id="e9f06-114">Если вы не хотите включать тип в модель, его можно исключить:</span><span class="sxs-lookup"><span data-stu-id="e9f06-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="e9f06-115">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="e9f06-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="e9f06-116">Текучий API</span><span class="sxs-lookup"><span data-stu-id="e9f06-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

### <a name="excluding-from-migrations"></a><span data-ttu-id="e9f06-117">Исключение из миграции</span><span class="sxs-lookup"><span data-stu-id="e9f06-117">Excluding from migrations</span></span>

> [!NOTE]
> <span data-ttu-id="e9f06-118">Возможность исключения таблиц из миграции была добавлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="e9f06-118">The ability to exclude tables from migrations was added in EF Core 5.0.</span></span>

<span data-ttu-id="e9f06-119">Иногда бывает полезно иметь один и тот же тип сущности, сопоставленный в нескольких `DbContext` типах.</span><span class="sxs-lookup"><span data-stu-id="e9f06-119">It is sometimes useful to have the same entity type mapped in multiple `DbContext` types.</span></span> <span data-ttu-id="e9f06-120">Это особенно справедливо при использовании [ограниченных](https://www.martinfowler.com/bliki/BoundedContext.html)контекстов, для которых обычно используется отдельный `DbContext` тип для каждого ограниченного контекста.</span><span class="sxs-lookup"><span data-stu-id="e9f06-120">This is especially true when using [bounded contexts](https://www.martinfowler.com/bliki/BoundedContext.html), for which it is common to have a different `DbContext` type for each bounded context.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableExcludeFromMigrations.cs?name=TableExcludeFromMigrations&highlight=4)]

<span data-ttu-id="e9f06-121">При такой миграции конфигурации таблица не создается `blogs` , но `Blog` по-прежнему включается в модель и может использоваться в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="e9f06-121">With this configuration migrations will not create the `blogs` table, but `Blog` is still included in the model and can be used normally.</span></span>

<span data-ttu-id="e9f06-122">Если необходимо приступить к управлению таблицей с помощью миграции, то следует создать новую миграцию, если `blogs` она не исключена.</span><span class="sxs-lookup"><span data-stu-id="e9f06-122">If you need to start managing the table using migrations again then a new migration should be created where `blogs` is not excluded.</span></span> <span data-ttu-id="e9f06-123">Следующая миграция теперь будет содержать все изменения, внесенные в таблицу.</span><span class="sxs-lookup"><span data-stu-id="e9f06-123">The next migration will now contain any changes made to the table.</span></span>

## <a name="table-name"></a><span data-ttu-id="e9f06-124">Имя таблицы</span><span class="sxs-lookup"><span data-stu-id="e9f06-124">Table name</span></span>

<span data-ttu-id="e9f06-125">По соглашению каждый тип сущности будет настроен для соотнесения с таблицей базы данных с тем же именем, что и свойство DbSet, предоставляющее сущность.</span><span class="sxs-lookup"><span data-stu-id="e9f06-125">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="e9f06-126">Если DbSet для данной сущности не существует, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="e9f06-126">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="e9f06-127">Вы можете вручную настроить имя таблицы:</span><span class="sxs-lookup"><span data-stu-id="e9f06-127">You can manually configure the table name:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="e9f06-128">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="e9f06-128">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="e9f06-129">Текучий API</span><span class="sxs-lookup"><span data-stu-id="e9f06-129">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="e9f06-130">Схема таблицы</span><span class="sxs-lookup"><span data-stu-id="e9f06-130">Table schema</span></span>

<span data-ttu-id="e9f06-131">При использовании реляционной базы данных таблицы — это соглашение, созданное в схеме базы данных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e9f06-131">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="e9f06-132">Например, Microsoft SQL Server будет использовать `dbo` схему (SQLite не поддерживает схемы).</span><span class="sxs-lookup"><span data-stu-id="e9f06-132">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="e9f06-133">Можно настроить создание таблиц в определенной схеме следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e9f06-133">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="e9f06-134">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="e9f06-134">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="e9f06-135">Текучий API</span><span class="sxs-lookup"><span data-stu-id="e9f06-135">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="e9f06-136">Вместо того чтобы указывать схему для каждой таблицы, можно также определить схему по умолчанию на уровне модели с помощью API-интерфейса Fluent:</span><span class="sxs-lookup"><span data-stu-id="e9f06-136">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="e9f06-137">Обратите внимание, что установка схемы по умолчанию также влияет на другие объекты базы данных, например последовательности.</span><span class="sxs-lookup"><span data-stu-id="e9f06-137">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>

## <a name="view-mapping"></a><span data-ttu-id="e9f06-138">Просмотр сопоставления</span><span class="sxs-lookup"><span data-stu-id="e9f06-138">View mapping</span></span>

<span data-ttu-id="e9f06-139">Типы сущностей можно сопоставлять с представлениями базы данных с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="e9f06-139">Entity types can be mapped to database views using the Fluent API.</span></span>

> [!Note]
> <span data-ttu-id="e9f06-140">EF предполагает, что представление, на которое имеется ссылка, уже существует в базе данных, оно не будет автоматически создаваться при миграции.</span><span class="sxs-lookup"><span data-stu-id="e9f06-140">EF will assume that the referenced view already exists in the database, it will not create it automatically in a migration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ViewNameAndSchema.cs?name=ViewNameAndSchema&highlight=1)]

 <span data-ttu-id="e9f06-141">Сопоставление с представлением приведет к удалению сопоставления таблицы по умолчанию, но тип сущности также можно сопоставить с таблицей явным образом.</span><span class="sxs-lookup"><span data-stu-id="e9f06-141">Mapping to a view will remove the default table mapping, but the entity type can also be mapped to a table explicitly.</span></span> <span data-ttu-id="e9f06-142">В этом случае для запросов будет использоваться сопоставление запросов, а для обновлений будут использоваться сопоставления таблиц.</span><span class="sxs-lookup"><span data-stu-id="e9f06-142">In this case the query mapping will be used for queries and the table mapping will be used for updates.</span></span>
