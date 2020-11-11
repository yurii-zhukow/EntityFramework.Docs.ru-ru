---
title: Свойства теневой копии и индексатора — EF Core
description: Настройка свойств тени и индексатора в модели Entity Framework Core
author: AndriySvyryd
ms.date: 10/09/2020
uid: core/modeling/shadow-properties
ms.openlocfilehash: 180478212b683a271d2519cc1a4c79be5d3f11b9
ms.sourcegitcommit: 42bbf7f68e92c364c5fff63092d3eb02229f568d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94503193"
---
# <a name="shadow-and-indexer-properties"></a><span data-ttu-id="e4bca-103">Свойства теневой копии и индексатора</span><span class="sxs-lookup"><span data-stu-id="e4bca-103">Shadow and Indexer Properties</span></span>

<span data-ttu-id="e4bca-104">Теневые свойства — это свойства, не определенные в классе сущности .NET, но определенные для этого типа сущности в модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="e4bca-104">Shadow properties are properties that aren't defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="e4bca-105">Значения и состояния этих свойств хранятся исключительно в средстве отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="e4bca-105">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="e4bca-106">Теневые свойства полезны при наличии в базе данных данных, которые не должны быть представлены в сопоставленных типах сущностей.</span><span class="sxs-lookup"><span data-stu-id="e4bca-106">Shadow properties are useful when there's data in the database that shouldn't be exposed on the mapped entity types.</span></span>

<span data-ttu-id="e4bca-107">Свойства индексатора — это свойства типа сущности, которые поддерживаются [индексатором](/dotnet/csharp/programming-guide/indexers/) в классе сущности .NET.</span><span class="sxs-lookup"><span data-stu-id="e4bca-107">Indexer properties are entity type properties, which are backed by an [indexer](/dotnet/csharp/programming-guide/indexers/) in .NET entity class.</span></span> <span data-ttu-id="e4bca-108">Доступ к ним можно получить с помощью индексатора в экземплярах класса .NET.</span><span class="sxs-lookup"><span data-stu-id="e4bca-108">They can be accessed using the indexer on the .NET class instances.</span></span> <span data-ttu-id="e4bca-109">Он также позволяет добавлять дополнительные свойства в тип сущности, не изменяя класс CLR.</span><span class="sxs-lookup"><span data-stu-id="e4bca-109">It also allows you to add additional properties to the entity type without changing the CLR class.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="e4bca-110">Свойства тени внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="e4bca-110">Foreign key shadow properties</span></span>

<span data-ttu-id="e4bca-111">Свойства теневой копии чаще всего используются для свойств внешнего ключа, где связь между двумя сущностями представляется значением внешнего ключа в базе данных, но Управление связью осуществляется на основе типов сущностей с помощью свойств навигации между типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="e4bca-111">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="e4bca-112">По соглашению EF будет представлять теневое свойство при обнаружении связи, но свойство внешнего ключа не найдено в зависимом классе сущности.</span><span class="sxs-lookup"><span data-stu-id="e4bca-112">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="e4bca-113">Свойству будет присвоено имя `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которая указывает на основную сущность, используется для именования).</span><span class="sxs-lookup"><span data-stu-id="e4bca-113">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="e4bca-114">Если имя свойства ключа участника включает имя свойства навигации, то имя будет иметь значение `<principal key property name>` .</span><span class="sxs-lookup"><span data-stu-id="e4bca-114">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="e4bca-115">Если в зависимой сущности нет свойства навигации, в его месте используется имя типа участника.</span><span class="sxs-lookup"><span data-stu-id="e4bca-115">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="e4bca-116">Например, приведенный ниже листинг кода приведет к тому, что в `BlogId` сущность будет введено свойство Shadow `Post` :</span><span class="sxs-lookup"><span data-stu-id="e4bca-116">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="e4bca-117">Настройка теневых свойств</span><span class="sxs-lookup"><span data-stu-id="e4bca-117">Configuring shadow properties</span></span>

<span data-ttu-id="e4bca-118">Для настройки теневых свойств можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="e4bca-118">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="e4bca-119">После вызова перегрузки строки `Property` можно связать любой из вызовов конфигурации для других свойств.</span><span class="sxs-lookup"><span data-stu-id="e4bca-119">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="e4bca-120">В следующем примере, поскольку не `Blog` имеет свойства CLR с именем `LastUpdated` , создается свойство Shadow:</span><span class="sxs-lookup"><span data-stu-id="e4bca-120">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="e4bca-121">Если имя, передаваемое в `Property` метод, совпадает с именем существующего свойства (теневого или определенного для класса сущностей), то код настроит это существующее свойство, а не создаст новое свойство Shadow.</span><span class="sxs-lookup"><span data-stu-id="e4bca-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="e4bca-122">Доступ к теневым свойствам</span><span class="sxs-lookup"><span data-stu-id="e4bca-122">Accessing shadow properties</span></span>

<span data-ttu-id="e4bca-123">Значения теневых свойств можно получить и изменить с помощью `ChangeTracker` API:</span><span class="sxs-lookup"><span data-stu-id="e4bca-123">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

```csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="e4bca-124">На теневые свойства можно ссылаться в запросах LINQ с помощью `EF.Property` статического метода:</span><span class="sxs-lookup"><span data-stu-id="e4bca-124">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

```csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

<span data-ttu-id="e4bca-125">Доступ к теневым свойствам после неотслеживаемого запроса невозможен, так как отслеживание изменений не отслеживает возвращаемые сущности.</span><span class="sxs-lookup"><span data-stu-id="e4bca-125">Shadow properties cannot be accessed after a no-tracking query since the entities returned are not tracked by the change tracker.</span></span>

## <a name="configuring-indexer-properties"></a><span data-ttu-id="e4bca-126">Настройка свойств индексатора</span><span class="sxs-lookup"><span data-stu-id="e4bca-126">Configuring indexer properties</span></span>

<span data-ttu-id="e4bca-127">Для настройки свойств индексатора можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="e4bca-127">You can use the Fluent API to configure indexer properties.</span></span> <span data-ttu-id="e4bca-128">После вызова метода `IndexerProperty` можно связать любой из вызовов конфигурации для других свойств.</span><span class="sxs-lookup"><span data-stu-id="e4bca-128">Once you've called the method `IndexerProperty`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="e4bca-129">В следующем примере `Blog` определен индексатор, который будет использоваться для создания свойства индексатора.</span><span class="sxs-lookup"><span data-stu-id="e4bca-129">In the following sample, `Blog` has an indexer defined and it will be used to create an indexer property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexerProperty.cs?name=ShadowProperty&highlight=3)]

<span data-ttu-id="e4bca-130">Если имя, передаваемое в `IndexerProperty` метод, совпадает с именем существующего свойства индексатора, то код настроит это существующее свойство.</span><span class="sxs-lookup"><span data-stu-id="e4bca-130">If the name supplied to the `IndexerProperty` method matches the name of an existing indexer property, then the code will configure that existing property.</span></span> <span data-ttu-id="e4bca-131">Если тип сущности имеет свойство, которое поддерживается свойством класса сущностей, возникает исключение, так как свойства индексатора должны быть доступны только через индексатор.</span><span class="sxs-lookup"><span data-stu-id="e4bca-131">If the entity type has a property, which is backed by a property on the entity class, then an exception is thrown since indexer properties must only be accessed via the indexer.</span></span>

## <a name="property-bag-entity-types"></a><span data-ttu-id="e4bca-132">Типы сущностей контейнера свойств</span><span class="sxs-lookup"><span data-stu-id="e4bca-132">Property bag entity types</span></span>

> [!NOTE]
> <span data-ttu-id="e4bca-133">В EF Core 5,0 добавлена поддержка типов сущностей контейнера свойств.</span><span class="sxs-lookup"><span data-stu-id="e4bca-133">Support for Property bag entity types was added in EF Core 5.0.</span></span>

<span data-ttu-id="e4bca-134">Типы сущностей, которые содержат только свойства индексатора, называются типами сущностей контейнера свойств.</span><span class="sxs-lookup"><span data-stu-id="e4bca-134">Entity types that contain only indexer properties are known as property bag entity types.</span></span> <span data-ttu-id="e4bca-135">Эти типы сущностей не имеют теневых свойств, а EF создаст свойства индексатора.</span><span class="sxs-lookup"><span data-stu-id="e4bca-135">These entity types don't have shadow properties, instead EF will create indexer properties.</span></span> <span data-ttu-id="e4bca-136">Сейчас `Dictionary<string, object>` поддерживается только в качестве типа сущности контейнера свойств.</span><span class="sxs-lookup"><span data-stu-id="e4bca-136">Currently only `Dictionary<string, object>` is supported as a property bag entity type.</span></span> <span data-ttu-id="e4bca-137">Его необходимо настроить в качестве общего типа сущности с уникальным именем, а соответствующее `DbSet` свойство должно быть реализовано с помощью `Set` вызова.</span><span class="sxs-lookup"><span data-stu-id="e4bca-137">It must be configured as a shared entity type with a unique name and the corresponding `DbSet` property must be implemented using a `Set` call.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedType.cs?name=SharedType&highlight=3,7)]
