---
title: Свойства тени — EF Core
description: Настройка теневых свойств в модели Entity Framework Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 06dbba7a4f6da247e6afde96debb0762fa55ce7e
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616716"
---
# <a name="shadow-properties"></a><span data-ttu-id="42c4d-103">Свойства тени</span><span class="sxs-lookup"><span data-stu-id="42c4d-103">Shadow Properties</span></span>

<span data-ttu-id="42c4d-104">Теневые свойства — это свойства, которые не определены в классе сущности .NET, но определены для этого типа сущности в модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="42c4d-104">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="42c4d-105">Значения и состояния этих свойств хранятся исключительно в средстве отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="42c4d-105">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="42c4d-106">Теневые свойства полезны при наличии в базе данных данных, которые не должны быть представлены в сопоставленных типах сущностей.</span><span class="sxs-lookup"><span data-stu-id="42c4d-106">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="42c4d-107">Свойства тени внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="42c4d-107">Foreign key shadow properties</span></span>

<span data-ttu-id="42c4d-108">Свойства теневой копии чаще всего используются для свойств внешнего ключа, где связь между двумя сущностями представляется значением внешнего ключа в базе данных, но Управление связью осуществляется на основе типов сущностей с помощью свойств навигации между типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="42c4d-108">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="42c4d-109">По соглашению EF будет представлять теневое свойство при обнаружении связи, но свойство внешнего ключа не найдено в зависимом классе сущности.</span><span class="sxs-lookup"><span data-stu-id="42c4d-109">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="42c4d-110">Свойству будет присвоено имя `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которая указывает на основную сущность, используется для именования).</span><span class="sxs-lookup"><span data-stu-id="42c4d-110">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="42c4d-111">Если имя свойства ключа участника включает имя свойства навигации, то имя будет иметь значение `<principal key property name>` .</span><span class="sxs-lookup"><span data-stu-id="42c4d-111">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="42c4d-112">Если в зависимой сущности нет свойства навигации, в его месте используется имя типа участника.</span><span class="sxs-lookup"><span data-stu-id="42c4d-112">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="42c4d-113">Например, приведенный ниже листинг кода приведет к тому, что в `BlogId` сущность будет введено свойство Shadow `Post` :</span><span class="sxs-lookup"><span data-stu-id="42c4d-113">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="42c4d-114">Настройка теневых свойств</span><span class="sxs-lookup"><span data-stu-id="42c4d-114">Configuring shadow properties</span></span>

<span data-ttu-id="42c4d-115">Для настройки теневых свойств можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="42c4d-115">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="42c4d-116">После вызова перегрузки строки `Property` можно связать любой из вызовов конфигурации для других свойств.</span><span class="sxs-lookup"><span data-stu-id="42c4d-116">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="42c4d-117">В следующем примере, поскольку не `Blog` имеет свойства CLR с именем `LastUpdated` , создается свойство Shadow:</span><span class="sxs-lookup"><span data-stu-id="42c4d-117">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="42c4d-118">Если имя, передаваемое в `Property` метод, совпадает с именем существующего свойства (теневого или определенного для класса сущностей), то код настроит это существующее свойство, а не создаст новое свойство Shadow.</span><span class="sxs-lookup"><span data-stu-id="42c4d-118">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="42c4d-119">Доступ к теневым свойствам</span><span class="sxs-lookup"><span data-stu-id="42c4d-119">Accessing shadow properties</span></span>

<span data-ttu-id="42c4d-120">Значения теневых свойств можно получить и изменить с помощью `ChangeTracker` API:</span><span class="sxs-lookup"><span data-stu-id="42c4d-120">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="42c4d-121">На теневые свойства можно ссылаться в запросах LINQ с помощью `EF.Property` статического метода:</span><span class="sxs-lookup"><span data-stu-id="42c4d-121">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

<span data-ttu-id="42c4d-122">Доступ к теневым свойствам после неотслеживаемого запроса невозможен, так как отслеживание изменений не отслеживает возвращаемые сущности.</span><span class="sxs-lookup"><span data-stu-id="42c4d-122">Shadow properties cannot be accessed after a no-tracking query since the entities returned are not tracked by the change tracker.</span></span>
