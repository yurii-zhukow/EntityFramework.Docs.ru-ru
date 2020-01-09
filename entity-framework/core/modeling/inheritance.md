---
title: Наследование — EF Core
description: Настройка наследования типов сущностей с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502176"
---
# <a name="inheritance"></a><span data-ttu-id="04988-103">Наследование</span><span class="sxs-lookup"><span data-stu-id="04988-103">Inheritance</span></span>

<span data-ttu-id="04988-104">EF может сопоставлять иерархию типов .NET с базой данных.</span><span class="sxs-lookup"><span data-stu-id="04988-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="04988-105">Это позволяет создавать сущности .NET в коде обычным образом, используя базовые и производные типы и позволяя EF легко создавать соответствующую схему базы данных, выдавать запросы и т. д. Фактические сведения о сопоставлении иерархии типов являются зависимыми от поставщика; на этой странице описывается поддержка наследования в контексте реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="04988-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="04988-106">В данный момент EF Core поддерживает только шаблон «одна таблица на иерархию».</span><span class="sxs-lookup"><span data-stu-id="04988-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="04988-107">Функция «подтаблица» использует одну таблицу для хранения данных всех типов в иерархии, а столбец дискриминатора используется для указания типа, представляемого каждой строкой.</span><span class="sxs-lookup"><span data-stu-id="04988-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="04988-108">Тип "Таблица — один-типа" (TPT) и "табличный/конкретный" тип (TPC), которые поддерживаются EF6, пока не поддерживаются EF Core.</span><span class="sxs-lookup"><span data-stu-id="04988-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="04988-109">TPT — это важный компонент, который планируется EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="04988-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="04988-110">Сопоставление иерархии типов сущностей</span><span class="sxs-lookup"><span data-stu-id="04988-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="04988-111">По соглашению EF настраивает наследование только в том случае, если в модель явно включены два или более унаследованных типа.</span><span class="sxs-lookup"><span data-stu-id="04988-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="04988-112">EF не будет автоматически сканировать базовые или производные типы, которые не включены в модель иным образом.</span><span class="sxs-lookup"><span data-stu-id="04988-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="04988-113">В модель можно включить типы, предоставив DbSet для каждого типа в иерархии наследования:</span><span class="sxs-lookup"><span data-stu-id="04988-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="04988-114">Эта модель должна быть сопоставлена со следующей схемой базы данных (Обратите внимание на неявно созданный столбец *дискриминатора* , который определяет, какой тип *блога* хранится в каждой строке):</span><span class="sxs-lookup"><span data-stu-id="04988-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![изображение](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="04988-116">Столбцы базы данных автоматически допускают значение null при необходимости при использовании сопоставления «одна таблица».</span><span class="sxs-lookup"><span data-stu-id="04988-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="04988-117">Например, столбец *рссурл* допускает значение null, так как обычные экземпляры *блога* не имеют этого свойства.</span><span class="sxs-lookup"><span data-stu-id="04988-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="04988-118">Если вы не хотите предоставлять DbSet для одной или нескольких сущностей в иерархии, можно также использовать API Fluent, чтобы убедиться, что они включены в модель.</span><span class="sxs-lookup"><span data-stu-id="04988-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="04988-119">Если вы не полагаетесь на соглашения, можно явно указать базовый тип с помощью `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="04988-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="04988-120">Можно также использовать `.HasBaseType((Type)null)` для удаления типа сущности из иерархии.</span><span class="sxs-lookup"><span data-stu-id="04988-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="04988-121">Конфигурация дискриминатора</span><span class="sxs-lookup"><span data-stu-id="04988-121">Discriminator configuration</span></span>

<span data-ttu-id="04988-122">Можно настроить имя и тип столбца дискриминатора, а также значения, которые используются для обнаружения каждого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="04988-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="04988-123">В приведенных выше примерах EF добавил дискриминатор неявно в качестве [Свойства теневой](xref:core/modeling/shadow-properties) копии базовой сущности иерархии.</span><span class="sxs-lookup"><span data-stu-id="04988-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="04988-124">Это свойство можно настроить так же, как и любое другое:</span><span class="sxs-lookup"><span data-stu-id="04988-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="04988-125">Наконец, дискриминатор также можно сопоставить с обычным свойством .NET в сущности:</span><span class="sxs-lookup"><span data-stu-id="04988-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="04988-126">Общие столбцы</span><span class="sxs-lookup"><span data-stu-id="04988-126">Shared columns</span></span>

<span data-ttu-id="04988-127">По умолчанию, если два одноуровневого типа сущности в иерархии имеют свойство с одинаковым именем, они будут сопоставлены с двумя отдельными столбцами.</span><span class="sxs-lookup"><span data-stu-id="04988-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="04988-128">Однако, если их тип идентичен, они могут быть сопоставлены с одним столбцом базы данных:</span><span class="sxs-lookup"><span data-stu-id="04988-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
