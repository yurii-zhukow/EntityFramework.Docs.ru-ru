---
title: Свойства тени — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414713"
---
# <a name="shadow-properties"></a><span data-ttu-id="e690d-102">Свойства тени</span><span class="sxs-lookup"><span data-stu-id="e690d-102">Shadow Properties</span></span>

<span data-ttu-id="e690d-103">Теневые свойства — это свойства, которые не определены в классе сущности .NET, но определены для этого типа сущности в модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="e690d-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="e690d-104">Значения и состояния этих свойств хранятся исключительно в средстве отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="e690d-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="e690d-105">Теневые свойства полезны при наличии в базе данных данных, которые не должны быть представлены в сопоставленных типах сущностей.</span><span class="sxs-lookup"><span data-stu-id="e690d-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="e690d-106">Свойства тени внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="e690d-106">Foreign key shadow properties</span></span>

<span data-ttu-id="e690d-107">Свойства теневой копии чаще всего используются для свойств внешнего ключа, где связь между двумя сущностями представляется значением внешнего ключа в базе данных, но Управление связью осуществляется на основе типов сущностей с помощью свойств навигации между сущностью. типов.</span><span class="sxs-lookup"><span data-stu-id="e690d-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="e690d-108">По соглашению EF будет представлять теневое свойство при обнаружении связи, но свойство внешнего ключа не найдено в зависимом классе сущности.</span><span class="sxs-lookup"><span data-stu-id="e690d-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="e690d-109">Свойство будет называться `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которая указывает на основную сущность, используется для именования).</span><span class="sxs-lookup"><span data-stu-id="e690d-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="e690d-110">Если имя свойства ключа участника содержит имя свойства навигации, то имя будет просто `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="e690d-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="e690d-111">Если в зависимой сущности нет свойства навигации, в его месте используется имя типа участника.</span><span class="sxs-lookup"><span data-stu-id="e690d-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="e690d-112">Например, приведенный ниже листинг кода приведет к тому, что в сущность `Post` будет введено свойство `BlogId` Shadow:</span><span class="sxs-lookup"><span data-stu-id="e690d-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="e690d-113">Настройка теневых свойств</span><span class="sxs-lookup"><span data-stu-id="e690d-113">Configuring shadow properties</span></span>

<span data-ttu-id="e690d-114">Для настройки теневых свойств можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="e690d-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="e690d-115">После вызова перегрузки строки `Property`можно связать любой из вызовов конфигурации для других свойств.</span><span class="sxs-lookup"><span data-stu-id="e690d-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="e690d-116">В следующем примере, поскольку `Blog` не имеет свойства CLR с именем `LastUpdated`, создается свойство Shadow:</span><span class="sxs-lookup"><span data-stu-id="e690d-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="e690d-117">Если имя, передаваемое в метод `Property`, совпадает с именем существующего свойства (теневого или определенного для класса сущностей), то код настроит это существующее свойство, а не создаст новое свойство Shadow.</span><span class="sxs-lookup"><span data-stu-id="e690d-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="e690d-118">Доступ к теневым свойствам</span><span class="sxs-lookup"><span data-stu-id="e690d-118">Accessing shadow properties</span></span>

<span data-ttu-id="e690d-119">Значения теневых свойств можно получить и изменить с помощью `ChangeTracker` API:</span><span class="sxs-lookup"><span data-stu-id="e690d-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="e690d-120">На теневые свойства в запросах LINQ можно ссылаться с помощью статического метода `EF.Property`:</span><span class="sxs-lookup"><span data-stu-id="e690d-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
