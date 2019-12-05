---
title: Ключи (основной) — EF Core
description: Настройка ключей для типов сущностей при использовании Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824624"
---
# <a name="keys-primary"></a><span data-ttu-id="b0bf7-103">Ключи (первичные)</span><span class="sxs-lookup"><span data-stu-id="b0bf7-103">Keys (primary)</span></span>

<span data-ttu-id="b0bf7-104">Ключ служит в качестве первичного уникального идентификатора для каждого экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="b0bf7-105">При использовании реляционной базы данных эта схема сопоставляется с понятием *первичного ключа*.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="b0bf7-106">Можно также настроить уникальный идентификатор, который не является первичным ключом (Дополнительные сведения см. в разделе [альтернативные ключи](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="b0bf7-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="b0bf7-107">Для настройки или создания первичного ключа можно использовать один из следующих методов.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="b0bf7-108">Обозначения</span><span class="sxs-lookup"><span data-stu-id="b0bf7-108">Conventions</span></span>

<span data-ttu-id="b0bf7-109">По умолчанию свойство с именем `Id` или `<type name>Id` будет настроено в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="b0bf7-110">[Принадлежащие типы сущностей](xref:core/modeling/owned-entities) используют разные правила для определения ключей.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b0bf7-111">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="b0bf7-111">Data Annotations</span></span>

<span data-ttu-id="b0bf7-112">Заметки к данным можно использовать для настройки одного свойства в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="b0bf7-113">Текучий API</span><span class="sxs-lookup"><span data-stu-id="b0bf7-113">Fluent API</span></span>

<span data-ttu-id="b0bf7-114">API-интерфейс Fluent можно использовать для настройки одного свойства в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="b0bf7-115">Можно также использовать API Fluent, чтобы настроить несколько свойств в качестве ключа сущности (называемого составным ключом).</span><span class="sxs-lookup"><span data-stu-id="b0bf7-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="b0bf7-116">Составные ключи могут быть настроены только с помощью правил API Fluent, но не будут настраивать составной ключ, и вы не сможете использовать заметки к данным для их настройки.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="b0bf7-117">Типы и значения ключей</span><span class="sxs-lookup"><span data-stu-id="b0bf7-117">Key types and values</span></span>

<span data-ttu-id="b0bf7-118">EF Core поддерживает использование свойств любого типа-примитива в качестве первичного ключа, включая `string`, `Guid`, `byte[]` и другие.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="b0bf7-119">Но не все базы данных поддерживают их.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-119">But not all databases support them.</span></span> <span data-ttu-id="b0bf7-120">В некоторых случаях значения ключей можно преобразовать в поддерживаемый тип автоматически, в противном случае преобразование должно быть [указано вручную](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="b0bf7-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="b0bf7-121">Ключевые свойства всегда должны иметь значение, отличное от значения по умолчанию, при добавлении новой сущности в контекст, но некоторые типы будут [созданы базой данных](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="b0bf7-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="b0bf7-122">В этом случае EF попытается создать временное значение при добавлении сущности в целях отслеживания.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="b0bf7-123">После вызова команды [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) временное значение будет заменено значением, созданным базой данных.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="b0bf7-124">Если ключевое свойство имеет значение, сформированное базой данных, и при добавлении сущности указано значение, отличное от значения по умолчанию, то EF предполагает, что сущность уже существует в базе данных и попытается обновить ее вместо вставки новой.</span><span class="sxs-lookup"><span data-stu-id="b0bf7-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="b0bf7-125">Чтобы избежать этого, не отключайте создание значений или Узнайте, [как указать явные значения для создаваемых свойств](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="b0bf7-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>