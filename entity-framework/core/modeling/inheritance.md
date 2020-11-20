---
title: Наследование — EF Core
description: Настройка наследования типов сущностей с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 10/01/2020
uid: core/modeling/inheritance
ms.openlocfilehash: 33429bbc4a9941ff8ea98a8f99cc652c8ea26455
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003631"
---
# <a name="inheritance"></a><span data-ttu-id="40f4d-103">Наследование</span><span class="sxs-lookup"><span data-stu-id="40f4d-103">Inheritance</span></span>

<span data-ttu-id="40f4d-104">EF может сопоставлять иерархию типов .NET с базой данных.</span><span class="sxs-lookup"><span data-stu-id="40f4d-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="40f4d-105">Это позволяет создавать сущности .NET в коде обычным образом, используя базовые и производные типы и позволяя EF легко создавать соответствующую схему базы данных, выдавать запросы и т. д. Фактические сведения о сопоставлении иерархии типов являются зависимыми от поставщика; на этой странице описывается поддержка наследования в контексте реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="40f4d-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="40f4d-106">Сопоставление иерархии типов сущностей</span><span class="sxs-lookup"><span data-stu-id="40f4d-106">Entity type hierarchy mapping</span></span>

<span data-ttu-id="40f4d-107">По соглашению EF не будет автоматически сканировать базовые или производные типы; Это означает, что если требуется сопоставить тип CLR в иерархии, необходимо явно указать этот тип в модели.</span><span class="sxs-lookup"><span data-stu-id="40f4d-107">By convention, EF will not automatically scan for base or derived types; this means that if you want a CLR type in your hierarchy to be mapped, you must explicitly specify that type on your model.</span></span> <span data-ttu-id="40f4d-108">Например, указание только базового типа иерархии не приведет к неявному включению всех его вложенных типов EF Core.</span><span class="sxs-lookup"><span data-stu-id="40f4d-108">For example, specifying only the base type of a hierarchy will not cause EF Core to implicitly include all of its sub-types.</span></span>

<span data-ttu-id="40f4d-109">В следующем примере предоставляется DbSet для `Blog` и его подкласс `RssBlog` .</span><span class="sxs-lookup"><span data-stu-id="40f4d-109">The following sample exposes a DbSet for `Blog` and its subclass `RssBlog`.</span></span> <span data-ttu-id="40f4d-110">Если `Blog` имеет любой другой подкласс, он не будет включаться в модель.</span><span class="sxs-lookup"><span data-stu-id="40f4d-110">If `Blog` has any other subclass, it will not be included in the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="40f4d-111">Столбцы базы данных автоматически допускают значение null при необходимости при использовании сопоставления «одна таблица».</span><span class="sxs-lookup"><span data-stu-id="40f4d-111">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="40f4d-112">Например, `RssUrl` столбец допускает значение null, так как у обычных `Blog` экземпляров нет этого свойства.</span><span class="sxs-lookup"><span data-stu-id="40f4d-112">For example, the `RssUrl` column is nullable because regular `Blog` instances do not have that property.</span></span>

<span data-ttu-id="40f4d-113">Если вы не хотите предоставлять `DbSet` для одной или нескольких сущностей в иерархии, можно также использовать API Fluent, чтобы убедиться, что они включены в модель.</span><span class="sxs-lookup"><span data-stu-id="40f4d-113">If you don't want to expose a `DbSet` for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="40f4d-114">Если вы не полагаетесь на соглашения, можно явно указать базовый тип с помощью `HasBaseType` .</span><span class="sxs-lookup"><span data-stu-id="40f4d-114">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="40f4d-115">Также можно использовать `.HasBaseType((Type)null)` для удаления типа сущности из иерархии.</span><span class="sxs-lookup"><span data-stu-id="40f4d-115">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="table-per-hierarchy-and-discriminator-configuration"></a><span data-ttu-id="40f4d-116">Конфигурация таблиц на иерархию и дискриминатор</span><span class="sxs-lookup"><span data-stu-id="40f4d-116">Table-per-hierarchy and discriminator configuration</span></span>

<span data-ttu-id="40f4d-117">По умолчанию EF сопоставляет наследование, используя шаблон «одна *таблица на иерархию* ».</span><span class="sxs-lookup"><span data-stu-id="40f4d-117">By default, EF maps the inheritance using the *table-per-hierarchy* (TPH) pattern.</span></span> <span data-ttu-id="40f4d-118">Функция «подтаблица» использует одну таблицу для хранения данных всех типов в иерархии, а столбец дискриминатора используется для указания типа, представляемого каждой строкой.</span><span class="sxs-lookup"><span data-stu-id="40f4d-118">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="40f4d-119">Приведенная выше модель сопоставлена со следующей схемой базы данных (Обратите внимание на `Discriminator` столбец, созданный неявно, который определяет, какой тип `Blog` хранится в каждой строке).</span><span class="sxs-lookup"><span data-stu-id="40f4d-119">The model above is mapped to the following database schema (note the implicitly created `Discriminator` column, which identifies which type of `Blog` is stored in each row).</span></span>

![Снимок экрана: результаты запроса иерархии сущностей блога с использованием шаблона «таблица на иерархию»](_static/inheritance-tph-data.png)

<span data-ttu-id="40f4d-121">Можно настроить имя и тип столбца дискриминатора, а также значения, которые используются для обнаружения каждого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="40f4d-121">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="40f4d-122">В приведенных выше примерах EF добавил дискриминатор неявно в качестве [Свойства теневой](xref:core/modeling/shadow-properties) копии базовой сущности иерархии.</span><span class="sxs-lookup"><span data-stu-id="40f4d-122">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="40f4d-123">Это свойство можно настроить так же, как и любое другое:</span><span class="sxs-lookup"><span data-stu-id="40f4d-123">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="40f4d-124">Наконец, дискриминатор также можно сопоставить с обычным свойством .NET в сущности:</span><span class="sxs-lookup"><span data-stu-id="40f4d-124">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

<span data-ttu-id="40f4d-125">При запросе производных сущностей, использующих шаблон «подстановка», EF Core добавляет предикат к столбцу дискриминатора в запросе.</span><span class="sxs-lookup"><span data-stu-id="40f4d-125">When querying for derived entities, which use the TPH pattern, EF Core adds a predicate over discriminator column in the query.</span></span> <span data-ttu-id="40f4d-126">Этот фильтр гарантирует, что мы не получаем дополнительные строки для базовых типов или родственных типов, отсутствующих в результатах.</span><span class="sxs-lookup"><span data-stu-id="40f4d-126">This filter makes sure that we don't get any additional rows for base types or sibling types not in the result.</span></span> <span data-ttu-id="40f4d-127">Этот предикат фильтра пропущен для базового типа сущности, так как при выполнении запроса для базовой сущности будут получены результаты для всех сущностей в иерархии.</span><span class="sxs-lookup"><span data-stu-id="40f4d-127">This filter predicate is skipped for the base entity type since querying for the base entity will get results for all the entities in the hierarchy.</span></span> <span data-ttu-id="40f4d-128">При материализации результатов из запроса, если мы получаем значение дискриминатора, которое не сопоставлено ни с одним из типов сущностей в модели, создается исключение, поскольку неизвестно, как материализовать результаты.</span><span class="sxs-lookup"><span data-stu-id="40f4d-128">When materializing results from a query, if we come across a discriminator value, which isn't mapped to any entity type in the model, we throw an exception since we don't know how to materialize the results.</span></span> <span data-ttu-id="40f4d-129">Эта ошибка возникает только в том случае, если база данных содержит строки со значениями дискриминатора, которые не сопоставлены в модели EF.</span><span class="sxs-lookup"><span data-stu-id="40f4d-129">This error only occurs if your database contains rows with discriminator values, which aren't mapped in the EF model.</span></span> <span data-ttu-id="40f4d-130">Если у вас есть такие данные, можно пометить дискриминатор в EF Core модели как неполное, чтобы указать, что необходимо всегда добавлять предикат фильтра для запроса любого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="40f4d-130">If you have such data, then you can mark the discriminator mapping in EF Core model as incomplete to indicate that we should always add filter predicate for querying any type in the hierarchy.</span></span> <span data-ttu-id="40f4d-131">`IsComplete(false)` вызов в конфигурации дискриминатора помечает сопоставление как неполное.</span><span class="sxs-lookup"><span data-stu-id="40f4d-131">`IsComplete(false)` call on the discriminator configuration marks the mapping to be incomplete.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorMappingIncomplete.cs?name=DiscriminatorMappingIncomplete&highlight=5)]

### <a name="shared-columns"></a><span data-ttu-id="40f4d-132">Общие столбцы</span><span class="sxs-lookup"><span data-stu-id="40f4d-132">Shared columns</span></span>

<span data-ttu-id="40f4d-133">По умолчанию, если два одноуровневого типа сущности в иерархии имеют свойство с одинаковым именем, они будут сопоставлены с двумя отдельными столбцами.</span><span class="sxs-lookup"><span data-stu-id="40f4d-133">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="40f4d-134">Однако, если их тип идентичен, они могут быть сопоставлены с одним столбцом базы данных:</span><span class="sxs-lookup"><span data-stu-id="40f4d-134">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]

## <a name="table-per-type-configuration"></a><span data-ttu-id="40f4d-135">Конфигурация типа "один таблица на тип"</span><span class="sxs-lookup"><span data-stu-id="40f4d-135">Table-per-type configuration</span></span>

> [!NOTE]
> <span data-ttu-id="40f4d-136">Функция "Таблица для каждого типа" (TPT) была представлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="40f4d-136">The table-per-type (TPT) feature was introduced in EF Core 5.0.</span></span> <span data-ttu-id="40f4d-137">Тип данных "Таблица — конкретный" (TPC) поддерживается EF6, но еще не поддерживается EF Core.</span><span class="sxs-lookup"><span data-stu-id="40f4d-137">Table-per-concrete-type (TPC) is supported by EF6, but is not yet supported by EF Core.</span></span>

<span data-ttu-id="40f4d-138">В шаблоне сопоставления TPT все типы сопоставлены с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="40f4d-138">In the TPT mapping pattern, all the types are mapped to individual tables.</span></span> <span data-ttu-id="40f4d-139">Как свойства базового типа, так и свойства производного типа хранятся в таблице, сопоставленной с этим типом.</span><span class="sxs-lookup"><span data-stu-id="40f4d-139">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="40f4d-140">Таблицы, которые сопоставляются с производными типами, также хранят внешний ключ, который соединяет производную таблицу с базовой таблицей.</span><span class="sxs-lookup"><span data-stu-id="40f4d-140">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TPTConfiguration.cs?name=TPTConfiguration)]

<span data-ttu-id="40f4d-141">EF создаст следующую схему базы данных для указанной выше модели.</span><span class="sxs-lookup"><span data-stu-id="40f4d-141">EF will create the following database schema for the model above.</span></span>

```sql
CREATE TABLE [Blogs] (
    [BlogId] int NOT NULL IDENTITY,
    [Url] nvarchar(max) NULL,
    CONSTRAINT [PK_Blogs] PRIMARY KEY ([BlogId])
);

CREATE TABLE [RssBlogs] (
    [BlogId] int NOT NULL,
    [RssUrl] nvarchar(max) NULL,
    CONSTRAINT [PK_RssBlogs] PRIMARY KEY ([BlogId]),
    CONSTRAINT [FK_RssBlogs_Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [Blogs] ([BlogId]) ON DELETE NO ACTION
);
```

> [!NOTE]
> <span data-ttu-id="40f4d-142">При переименовании ограничения PRIMARY KEY новое имя будет применено ко всем таблицам, сопоставленным с иерархией, а будущие версии EF будут разрешать Переименование ограничения только для конкретной таблицы, если [проблема 19970](https://github.com/dotnet/efcore/issues/19970) исправлена.</span><span class="sxs-lookup"><span data-stu-id="40f4d-142">If the primary key constraint is renamed the new name will be applied to all the tables mapped to the hierarchy, future EF versions will allow renaming the constraint only for a particular table when [issue 19970](https://github.com/dotnet/efcore/issues/19970) is fixed.</span></span>

<span data-ttu-id="40f4d-143">При использовании групповой конфигурации можно получить имя столбца для определенной таблицы, вызвав метод <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName%2A> .</span><span class="sxs-lookup"><span data-stu-id="40f4d-143">If you are employing bulk configuration you can retrieve the column name for a specific table by calling <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName%2A>.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TPTConfiguration.cs?name=Metadata&highlight=10)]
