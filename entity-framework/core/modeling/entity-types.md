---
title: Типы сущностей — EF Core
description: Как настроить и сопоставлять типы сущностей с помощью Entity Framework Core
author: roji
ms.date: 10/06/2020
uid: core/modeling/entity-types
ms.openlocfilehash: 6c8029b64210dd9bc69fd53d9a3b4aa06bf519e0
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023995"
---
# <a name="entity-types"></a><span data-ttu-id="b82e2-103">Типы сущностей</span><span class="sxs-lookup"><span data-stu-id="b82e2-103">Entity Types</span></span>

<span data-ttu-id="b82e2-104">Включение DbSet типа в контекст означает, что он включен в модель EF Core; как правило, тип сущности упоминается как *сущность*.</span><span class="sxs-lookup"><span data-stu-id="b82e2-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="b82e2-105">EF Core может считывать и записывать экземпляры сущностей из базы данных, а также, если используется реляционная база данных, EF Core может создавать таблицы для сущностей с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="b82e2-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="b82e2-106">Включение типов в модель</span><span class="sxs-lookup"><span data-stu-id="b82e2-106">Including types in the model</span></span>

<span data-ttu-id="b82e2-107">По соглашению типы, предоставляемые в свойствах DbSet в контексте, включаются в модель в качестве сущностей.</span><span class="sxs-lookup"><span data-stu-id="b82e2-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="b82e2-108">Также включаются типы сущностей, указанные в `OnModelCreating` методе, как и любые типы, которые обнаруживаются рекурсивным просмотром свойств навигации других обнаруженных типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="b82e2-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="b82e2-109">В приведенном ниже примере кода включены все типы:</span><span class="sxs-lookup"><span data-stu-id="b82e2-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="b82e2-110">`Blog` включен, так как он предоставляется в свойстве DbSet в контексте.</span><span class="sxs-lookup"><span data-stu-id="b82e2-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="b82e2-111">`Post` включен, так как он обнаруживается через `Blog.Posts` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="b82e2-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="b82e2-112">`AuditEntry` так как он указан в `OnModelCreating` .</span><span class="sxs-lookup"><span data-stu-id="b82e2-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="b82e2-113">Исключение типов из модели</span><span class="sxs-lookup"><span data-stu-id="b82e2-113">Excluding types from the model</span></span>

<span data-ttu-id="b82e2-114">Если вы не хотите включать тип в модель, его можно исключить:</span><span class="sxs-lookup"><span data-stu-id="b82e2-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="b82e2-115">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="b82e2-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="b82e2-116">Текучий API</span><span class="sxs-lookup"><span data-stu-id="b82e2-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

### <a name="excluding-from-migrations"></a><span data-ttu-id="b82e2-117">Исключение из миграции</span><span class="sxs-lookup"><span data-stu-id="b82e2-117">Excluding from migrations</span></span>

> [!NOTE]
> <span data-ttu-id="b82e2-118">Возможность исключения таблиц из миграции была введена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="b82e2-118">The ability to exclude tables from migrations was introduced in EF Core 5.0.</span></span>

<span data-ttu-id="b82e2-119">Иногда бывает полезно иметь один и тот же тип сущности, сопоставленный в нескольких `DbContext` типах.</span><span class="sxs-lookup"><span data-stu-id="b82e2-119">It is sometimes useful to have the same entity type mapped in multiple `DbContext` types.</span></span> <span data-ttu-id="b82e2-120">Это особенно справедливо при использовании [ограниченных](https://www.martinfowler.com/bliki/BoundedContext.html)контекстов, для которых обычно используется отдельный `DbContext` тип для каждого ограниченного контекста.</span><span class="sxs-lookup"><span data-stu-id="b82e2-120">This is especially true when using [bounded contexts](https://www.martinfowler.com/bliki/BoundedContext.html), for which it is common to have a different `DbContext` type for each bounded context.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableExcludeFromMigrations.cs?name=TableExcludeFromMigrations&highlight=4)]

<span data-ttu-id="b82e2-121">При такой миграции конфигурации таблица не создается `AspNetUsers` , но `IdentityUser` по-прежнему включается в модель и может использоваться в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="b82e2-121">With this configuration migrations will not create the `AspNetUsers` table, but `IdentityUser` is still included in the model and can be used normally.</span></span>

<span data-ttu-id="b82e2-122">Если необходимо приступить к управлению таблицей с помощью миграции, то следует создать новую миграцию, если `AspNetUsers` она не исключена.</span><span class="sxs-lookup"><span data-stu-id="b82e2-122">If you need to start managing the table using migrations again then a new migration should be created where `AspNetUsers` is not excluded.</span></span> <span data-ttu-id="b82e2-123">Следующая миграция теперь будет содержать все изменения, внесенные в таблицу.</span><span class="sxs-lookup"><span data-stu-id="b82e2-123">The next migration will now contain any changes made to the table.</span></span>

## <a name="table-name"></a><span data-ttu-id="b82e2-124">Имя таблицы</span><span class="sxs-lookup"><span data-stu-id="b82e2-124">Table name</span></span>

<span data-ttu-id="b82e2-125">По соглашению каждый тип сущности будет настроен для соотнесения с таблицей базы данных с тем же именем, что и свойство DbSet, предоставляющее сущность.</span><span class="sxs-lookup"><span data-stu-id="b82e2-125">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="b82e2-126">Если DbSet для данной сущности не существует, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="b82e2-126">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="b82e2-127">Вы можете вручную настроить имя таблицы:</span><span class="sxs-lookup"><span data-stu-id="b82e2-127">You can manually configure the table name:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="b82e2-128">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="b82e2-128">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="b82e2-129">Текучий API</span><span class="sxs-lookup"><span data-stu-id="b82e2-129">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="b82e2-130">Схема таблицы</span><span class="sxs-lookup"><span data-stu-id="b82e2-130">Table schema</span></span>

<span data-ttu-id="b82e2-131">При использовании реляционной базы данных таблицы — это соглашение, созданное в схеме базы данных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b82e2-131">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="b82e2-132">Например, Microsoft SQL Server будет использовать `dbo` схему (SQLite не поддерживает схемы).</span><span class="sxs-lookup"><span data-stu-id="b82e2-132">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="b82e2-133">Можно настроить создание таблиц в определенной схеме следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b82e2-133">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="b82e2-134">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="b82e2-134">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="b82e2-135">Текучий API</span><span class="sxs-lookup"><span data-stu-id="b82e2-135">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="b82e2-136">Вместо того чтобы указывать схему для каждой таблицы, можно также определить схему по умолчанию на уровне модели с помощью API-интерфейса Fluent:</span><span class="sxs-lookup"><span data-stu-id="b82e2-136">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="b82e2-137">Обратите внимание, что установка схемы по умолчанию также влияет на другие объекты базы данных, например последовательности.</span><span class="sxs-lookup"><span data-stu-id="b82e2-137">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>

## <a name="view-mapping"></a><span data-ttu-id="b82e2-138">Просмотр сопоставления</span><span class="sxs-lookup"><span data-stu-id="b82e2-138">View mapping</span></span>

<span data-ttu-id="b82e2-139">Типы сущностей можно сопоставлять с представлениями базы данных с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b82e2-139">Entity types can be mapped to database views using the Fluent API.</span></span>

> [!Note]
> <span data-ttu-id="b82e2-140">EF предполагает, что представление, на которое имеется ссылка, уже существует в базе данных, оно не будет автоматически создаваться при миграции.</span><span class="sxs-lookup"><span data-stu-id="b82e2-140">EF will assume that the referenced view already exists in the database, it will not create it automatically in a migration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ViewNameAndSchema.cs?name=ViewNameAndSchema&highlight=1)]

 <span data-ttu-id="b82e2-141">Сопоставление с представлением приведет к удалению сопоставления таблицы по умолчанию, но начиная с EF 5,0 тип сущности также можно сопоставить с таблицей явным образом.</span><span class="sxs-lookup"><span data-stu-id="b82e2-141">Mapping to a view will remove the default table mapping, but starting with EF 5.0 the entity type can also be mapped to a table explicitly.</span></span> <span data-ttu-id="b82e2-142">В этом случае для запросов будет использоваться сопоставление запросов, а для обновлений будут использоваться сопоставления таблиц.</span><span class="sxs-lookup"><span data-stu-id="b82e2-142">In this case the query mapping will be used for queries and the table mapping will be used for updates.</span></span>

> [!TIP]
> <span data-ttu-id="b82e2-143">Чтобы проверить типы сущностей, сопоставленные с представлениями с помощью поставщика в памяти, сопоставьте их с запросом через `ToInMemoryQuery` .</span><span class="sxs-lookup"><span data-stu-id="b82e2-143">To test entity types mapped to views using the in-memory provider map them to a query via `ToInMemoryQuery`.</span></span> <span data-ttu-id="b82e2-144">Дополнительные сведения см. в разделе [готовый к запуску пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Testing/ItemsWebApi/) с использованием этого метода.</span><span class="sxs-lookup"><span data-stu-id="b82e2-144">See a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Testing/ItemsWebApi/) using this technique for more details.</span></span>

## <a name="table-valued-function-mapping"></a><span data-ttu-id="b82e2-145">Сопоставление функций, возвращающих табличное значение</span><span class="sxs-lookup"><span data-stu-id="b82e2-145">Table-valued function mapping</span></span>

<span data-ttu-id="b82e2-146">Можно сопоставлять тип сущности с возвращающей табличное значение функцией вместо таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b82e2-146">It's possible to map an entity type to a table-valued function (TVF) instead of a table in the database.</span></span> <span data-ttu-id="b82e2-147">Чтобы проиллюстрировать это, давайте определим другую сущность, которая представляет блог с несколькими записями.</span><span class="sxs-lookup"><span data-stu-id="b82e2-147">To illustrate this, let's define another entity that represents blog with multiple posts.</span></span> <span data-ttu-id="b82e2-148">В этом примере сущность [не имеет смысла, но](xref:core/modeling/keyless-entity-types)она не должна быть.</span><span class="sxs-lookup"><span data-stu-id="b82e2-148">In the example, the entity is [keyless](xref:core/modeling/keyless-entity-types), but it doesn't have to be.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs#BlogWithMultiplePostsEntity)]

<span data-ttu-id="b82e2-149">Затем создайте в базе данных следующую функцию с табличным значением, которая возвращает только блоги с несколькими записями, а также число записей, связанных с каждым из этих блогов:</span><span class="sxs-lookup"><span data-stu-id="b82e2-149">Next, create the following table-valued function in the database, which returns only blogs with multiple posts as well as the number of posts associated with each of these blogs:</span></span>

```sql
CREATE FUNCTION dbo.BlogsWithMultiplePosts()
RETURNS TABLE
AS
RETURN
(
    SELECT b.Url, COUNT(p.BlogId) AS PostCount
    FROM Blogs AS b
    JOIN Posts AS p ON b.BlogId = p.BlogId
    GROUP BY b.BlogId, b.Url
    HAVING COUNT(p.BlogId) > 1
)
```

<span data-ttu-id="b82e2-150">Теперь сущность `BlogWithMultiplePost` может быть сопоставлена с этой функцией следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b82e2-150">Now, the entity `BlogWithMultiplePost` can be mapped to this function in a following way:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs#QueryableFunctionConfigurationToFunction)]

> [!NOTE]
> <span data-ttu-id="b82e2-151">Чтобы связать сущность с возвращающей табличное значение функцией, функция должна быть без параметров.</span><span class="sxs-lookup"><span data-stu-id="b82e2-151">In order to map an entity to a table-valued function the function must be parameterless.</span></span>

<span data-ttu-id="b82e2-152">Согласно соглашению, свойства сущности будут сопоставляться с соответствующими столбцами, возвращаемыми функцией.</span><span class="sxs-lookup"><span data-stu-id="b82e2-152">Conventionally the entity properties will be mapped to matching columns returned by the TVF.</span></span> <span data-ttu-id="b82e2-153">Если столбцы, возвращаемые функцией "функция", имеют разные имена, чем свойство сущности, то их можно настроить с помощью `HasColumnName` метода, как и при сопоставлении с обычной таблицей.</span><span class="sxs-lookup"><span data-stu-id="b82e2-153">If the columns returned by TVF has different name than entity property then it can be configured using `HasColumnName` method, just like when mapping to a regular table.</span></span>

<span data-ttu-id="b82e2-154">Если тип сущности сопоставляется с функцией, возвращающей табличное значение, запрос:</span><span class="sxs-lookup"><span data-stu-id="b82e2-154">When the entity type is mapped to a table-valued function, the query:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Program.cs#ToFunctionQuery)]

<span data-ttu-id="b82e2-155">Преобразуется в следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="b82e2-155">Produces the following SQL:</span></span>

```sql
SELECT [b].[Url], [b].[PostCount]
FROM [dbo].[BlogsWithMultiplePosts]() AS [b]
WHERE [b].[PostCount] > 3
```

## <a name="table-comments"></a><span data-ttu-id="b82e2-156">Комментарии к таблице</span><span class="sxs-lookup"><span data-stu-id="b82e2-156">Table comments</span></span>

<span data-ttu-id="b82e2-157">Можно задать произвольный текстовый комментарий, заданный для таблицы базы данных, что позволит документировать схему в базе данных:</span><span class="sxs-lookup"><span data-stu-id="b82e2-157">You can set an arbitrary text comment that gets set on the database table, allowing you to document your schema in the database:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="b82e2-158">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="b82e2-158">Data Annotations</span></span>](#tab/data-annotations)

> [!NOTE]
> <span data-ttu-id="b82e2-159">Установка комментариев с помощью заметок к данным была представлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="b82e2-159">Setting comments via data annotations was introduced in EF Core 5.0.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableComment.cs?name=TableComment&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="b82e2-160">Текучий API</span><span class="sxs-lookup"><span data-stu-id="b82e2-160">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableComment.cs?name=TableComment&highlight=4)]

***
