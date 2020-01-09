---
title: Типы сущностей — EF Core
description: Как настроить и сопоставлять типы сущностей с помощью Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502417"
---
# <a name="entity-types"></a><span data-ttu-id="c0ef5-103">Типы сущности</span><span class="sxs-lookup"><span data-stu-id="c0ef5-103">Entity Types</span></span>

<span data-ttu-id="c0ef5-104">Включение DbSet типа в контекст означает, что он включен в модель EF Core; как правило, тип сущности упоминается как *сущность*.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="c0ef5-105">EF Core может считывать и записывать экземпляры сущностей из базы данных, а также, если используется реляционная база данных, EF Core может создавать таблицы для сущностей с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="c0ef5-106">Включение типов в модель</span><span class="sxs-lookup"><span data-stu-id="c0ef5-106">Including types in the model</span></span>

<span data-ttu-id="c0ef5-107">По соглашению типы, предоставляемые в свойствах DbSet в контексте, включаются в модель в качестве сущностей.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="c0ef5-108">Также включаются типы сущностей, указанные в методе `OnModelCreating`, как и любые типы, которые обнаруживаются рекурсивным просмотром свойств навигации других обнаруженных типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="c0ef5-109">В приведенном ниже примере кода включены все типы:</span><span class="sxs-lookup"><span data-stu-id="c0ef5-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="c0ef5-110">`Blog` включен, так как он предоставляется в свойстве DbSet в контексте.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="c0ef5-111">`Post` включен, так как он обнаруживается с помощью свойства навигации `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="c0ef5-112">`AuditEntry`, так как он указан в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="c0ef5-113">Исключение типов из модели</span><span class="sxs-lookup"><span data-stu-id="c0ef5-113">Excluding types from the model</span></span>

<span data-ttu-id="c0ef5-114">Если вы не хотите включать тип в модель, его можно исключить:</span><span class="sxs-lookup"><span data-stu-id="c0ef5-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="c0ef5-115">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="c0ef5-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="c0ef5-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c0ef5-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a><span data-ttu-id="c0ef5-117">Имя таблицы</span><span class="sxs-lookup"><span data-stu-id="c0ef5-117">Table name</span></span>

<span data-ttu-id="c0ef5-118">По соглашению каждый тип сущности будет настроен для соотнесения с таблицей базы данных с тем же именем, что и свойство DbSet, предоставляющее сущность.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-118">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="c0ef5-119">Если DbSet для данной сущности не существует, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-119">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="c0ef5-120">Вы можете вручную настроить имя таблицы:</span><span class="sxs-lookup"><span data-stu-id="c0ef5-120">You can manually configure the table name:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="c0ef5-121">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="c0ef5-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="c0ef5-122">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c0ef5-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="c0ef5-123">Схема таблицы</span><span class="sxs-lookup"><span data-stu-id="c0ef5-123">Table schema</span></span>

<span data-ttu-id="c0ef5-124">При использовании реляционной базы данных таблицы — это соглашение, созданное в схеме базы данных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-124">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="c0ef5-125">Например, Microsoft SQL Server будет использовать схему `dbo` (SQLite не поддерживает схемы).</span><span class="sxs-lookup"><span data-stu-id="c0ef5-125">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="c0ef5-126">Можно настроить создание таблиц в определенной схеме следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-126">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="c0ef5-127">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="c0ef5-127">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="c0ef5-128">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c0ef5-128">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="c0ef5-129">Вместо того чтобы указывать схему для каждой таблицы, можно также определить схему по умолчанию на уровне модели с помощью API-интерфейса Fluent:</span><span class="sxs-lookup"><span data-stu-id="c0ef5-129">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="c0ef5-130">Обратите внимание, что установка схемы по умолчанию также влияет на другие объекты базы данных, например последовательности.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-130">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>
