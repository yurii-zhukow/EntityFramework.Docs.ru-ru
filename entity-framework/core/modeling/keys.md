---
title: Ключи — EF Core
description: Настройка ключей для типов сущностей при использовании Entity Framework Core
author: AndriySvyryd
ms.date: 1/10/2021
uid: core/modeling/keys
ms.openlocfilehash: c79ab0445e80b0b6f4a8b49ef0d4c063bf938851
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983537"
---
# <a name="keys"></a><span data-ttu-id="1a471-103">Ключи</span><span class="sxs-lookup"><span data-stu-id="1a471-103">Keys</span></span>

<span data-ttu-id="1a471-104">Ключ служит уникальным идентификатором для каждого экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="1a471-104">A key serves as a unique identifier for each entity instance.</span></span> <span data-ttu-id="1a471-105">Большинство сущностей в EF имеют один ключ, который сопоставляется с понятием *первичного ключа* в реляционных базах данных (для сущностей без ключей см. раздел несвязные [сущности](xref:core/modeling/keyless-entity-types)).</span><span class="sxs-lookup"><span data-stu-id="1a471-105">Most entities in EF have a single key, which maps to the concept of a *primary key* in relational databases (for entities without keys, see [Keyless entities](xref:core/modeling/keyless-entity-types)).</span></span> <span data-ttu-id="1a471-106">Сущности могут иметь дополнительные ключи за пределами первичного ключа (Дополнительные сведения см. в разделе [альтернативные ключи](#alternate-keys) ).</span><span class="sxs-lookup"><span data-stu-id="1a471-106">Entities can have additional keys beyond the primary key (see [Alternate Keys](#alternate-keys) for more information).</span></span>

## <a name="configuring-a-primary-key"></a><span data-ttu-id="1a471-107">Настройка первичного ключа</span><span class="sxs-lookup"><span data-stu-id="1a471-107">Configuring a primary key</span></span>

<span data-ttu-id="1a471-108">По соглашению свойство с именем `Id` или `<type name>Id` будет настроено в качестве первичного ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="1a471-108">By convention, a property named `Id` or `<type name>Id` will be configured as the primary key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> <span data-ttu-id="1a471-109">[Принадлежащие типы сущностей](xref:core/modeling/owned-entities) используют разные правила для определения ключей.</span><span class="sxs-lookup"><span data-stu-id="1a471-109">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

<span data-ttu-id="1a471-110">Вы можете настроить одно свойство в качестве первичного ключа сущности следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1a471-110">You can configure a single property to be the primary key of an entity as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="1a471-111">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="1a471-111">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

### <a name="fluent-api"></a>[<span data-ttu-id="1a471-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="1a471-112">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

<span data-ttu-id="1a471-113">Можно также настроить несколько свойств в качестве ключа сущности. это называется составным ключом.</span><span class="sxs-lookup"><span data-stu-id="1a471-113">You can also configure multiple properties to be the key of an entity - this is known as a composite key.</span></span> <span data-ttu-id="1a471-114">Составные ключи можно настроить только с помощью API Fluent. соглашения никогда не настраивают составной ключ и не могут использовать заметки к данным для его настройки.</span><span class="sxs-lookup"><span data-stu-id="1a471-114">Composite keys can only be configured using the Fluent API; conventions will never set up a composite key, and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="value-generation"></a><span data-ttu-id="1a471-115">Создание ценностей</span><span class="sxs-lookup"><span data-stu-id="1a471-115">Value generation</span></span>

<span data-ttu-id="1a471-116">Для несоставных числовых и первичных ключей GUID EF Core настраивает создание значений по соглашению.</span><span class="sxs-lookup"><span data-stu-id="1a471-116">For non-composite numeric and GUID primary keys, EF Core sets up value generation for you by convention.</span></span> <span data-ttu-id="1a471-117">Например, числовой первичный ключ в SQL Server автоматически настраивается как столбец ИДЕНТИФИКАТОРов.</span><span class="sxs-lookup"><span data-stu-id="1a471-117">For example, a numeric primary key in SQL Server is automatically set up to be an IDENTITY column.</span></span> <span data-ttu-id="1a471-118">Дополнительные сведения см. [в документации по созданию значения](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="1a471-118">For more information, see [the documentation on value generation](xref:core/modeling/generated-properties).</span></span>

## <a name="primary-key-name"></a><span data-ttu-id="1a471-119">Имя первичного ключа</span><span class="sxs-lookup"><span data-stu-id="1a471-119">Primary key name</span></span>

<span data-ttu-id="1a471-120">По соглашению для первичных ключей реляционных баз данных создаются имена `PK_<type name>` .</span><span class="sxs-lookup"><span data-stu-id="1a471-120">By convention, on relational databases primary keys are created with the name `PK_<type name>`.</span></span> <span data-ttu-id="1a471-121">Имя ограничения первичного ключа можно настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1a471-121">You can configure the name of the primary key constraint as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a><span data-ttu-id="1a471-122">Типы и значения ключей</span><span class="sxs-lookup"><span data-stu-id="1a471-122">Key types and values</span></span>

<span data-ttu-id="1a471-123">Хотя EF Core поддерживает использование свойств любого типа-примитива в качестве первичного ключа, включая `string` , `Guid` `byte[]` и другие, не все базы данных поддерживают все типы в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="1a471-123">While EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others, not all databases support all types as keys.</span></span> <span data-ttu-id="1a471-124">В некоторых случаях значения ключей можно преобразовать в поддерживаемый тип автоматически, в противном случае преобразование должно быть [указано вручную](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="1a471-124">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="1a471-125">Ключевые свойства всегда должны иметь значение, отличное от значения по умолчанию, при добавлении новой сущности в контекст, но некоторые типы будут [созданы базой данных](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="1a471-125">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="1a471-126">В этом случае EF попытается создать временное значение при добавлении сущности в целях отслеживания.</span><span class="sxs-lookup"><span data-stu-id="1a471-126">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="1a471-127">После вызова команды [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) временное значение будет заменено значением, созданным базой данных.</span><span class="sxs-lookup"><span data-stu-id="1a471-127">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="1a471-128">Если ключевое свойство имеет значение, сформированное базой данных, и при добавлении сущности указано значение, отличное от значения по умолчанию, то EF предполагает, что сущность уже существует в базе данных и попытается обновить ее вместо вставки новой.</span><span class="sxs-lookup"><span data-stu-id="1a471-128">If a key property has its value generated by the database and a non-default value is specified when an entity is added, then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="1a471-129">Чтобы избежать этого, отключите создание значений или Узнайте, [как указать явные значения для создаваемых свойств](xref:core/modeling/generated-properties#overriding-value-generation).</span><span class="sxs-lookup"><span data-stu-id="1a471-129">To avoid this, turn off value generation or see [how to specify explicit values for generated properties](xref:core/modeling/generated-properties#overriding-value-generation).</span></span>

## <a name="alternate-keys"></a><span data-ttu-id="1a471-130">Альтернативные ключи</span><span class="sxs-lookup"><span data-stu-id="1a471-130">Alternate Keys</span></span>

<span data-ttu-id="1a471-131">Альтернативный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности в дополнение к первичному ключу. его можно использовать в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="1a471-131">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key; it can be used as the target of a relationship.</span></span> <span data-ttu-id="1a471-132">При использовании реляционной базы данных эта схема сопоставляется с концепцией уникального индекса или ограничения для альтернативных ключевых столбцов и одного или нескольких ограничений внешнего ключа, которые ссылаются на эти столбцы.</span><span class="sxs-lookup"><span data-stu-id="1a471-132">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]
> <span data-ttu-id="1a471-133">Если нужно просто обеспечить уникальность столбца, определите уникальный индекс, а не альтернативный ключ (см. раздел [индексы](xref:core/modeling/indexes)).</span><span class="sxs-lookup"><span data-stu-id="1a471-133">If you just want to enforce uniqueness on a column, define a unique index rather than an alternate key (see [Indexes](xref:core/modeling/indexes)).</span></span> <span data-ttu-id="1a471-134">В EF альтернативные ключи доступны только для чтения и обеспечивают дополнительную семантику для уникальных индексов, так как их можно использовать в качестве целевого объекта для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1a471-134">In EF, alternate keys are read-only and provide additional semantics over unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="1a471-135">Альтернативные ключи обычно вводятся при необходимости, и их не нужно настраивать вручную.</span><span class="sxs-lookup"><span data-stu-id="1a471-135">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="1a471-136">По соглашению альтернативный ключ вводится при определении свойства, которое не является первичным ключом в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="1a471-136">By convention, an alternate key is introduced for you when you identify a property which isn't the primary key as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

<span data-ttu-id="1a471-137">Можно также настроить одно свойство в качестве альтернативного ключа:</span><span class="sxs-lookup"><span data-stu-id="1a471-137">You can also configure a single property to be an alternate key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

<span data-ttu-id="1a471-138">Можно также настроить несколько свойств в качестве альтернативного ключа (называемого составным альтернативным ключом):</span><span class="sxs-lookup"><span data-stu-id="1a471-138">You can also configure multiple properties to be an alternate key (known as a composite alternate key):</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

<span data-ttu-id="1a471-139">Наконец, в соответствии с соглашением индекс и ограничение, введенные для альтернативного ключа, будут называться `AK_<type name>_<property name>` (для составных альтернативных ключей `<property name>` становится списком имен свойств, разделенных подчеркиванием).</span><span class="sxs-lookup"><span data-stu-id="1a471-139">Finally, by convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>` (for composite alternate keys `<property name>` becomes an underscore separated list of property names).</span></span> <span data-ttu-id="1a471-140">Можно настроить имя индекса альтернативного ключа и ограничения уникальности.</span><span class="sxs-lookup"><span data-stu-id="1a471-140">You can configure the name of the alternate key's index and unique constraint:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
