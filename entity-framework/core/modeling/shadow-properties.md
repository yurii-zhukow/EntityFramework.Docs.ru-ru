---
title: Свойства тени — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655953"
---
# <a name="shadow-properties"></a><span data-ttu-id="2d1c6-102">Свойства тени</span><span class="sxs-lookup"><span data-stu-id="2d1c6-102">Shadow Properties</span></span>

<span data-ttu-id="2d1c6-103">Теневые свойства — это свойства, которые не определены в классе сущности .NET, но определены для этого типа сущности в модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="2d1c6-104">Значения и состояния этих свойств хранятся исключительно в средстве отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="2d1c6-105">Теневые свойства полезны при наличии в базе данных данных, которые не должны быть представлены в сопоставленных типах сущностей.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="2d1c6-106">Они чаще всего используются для свойств внешнего ключа, где связь между двумя сущностями представлена значением внешнего ключа в базе данных, но Управление связью осуществляется на основе типов сущностей с помощью свойств навигации между типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="2d1c6-107">Значения теневых свойств можно получить и изменить с помощью `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="2d1c6-108">На теневые свойства в запросах LINQ можно ссылаться с помощью статического метода `EF.Property`.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="2d1c6-109">Соглашения</span><span class="sxs-lookup"><span data-stu-id="2d1c6-109">Conventions</span></span>

<span data-ttu-id="2d1c6-110">Теневые свойства могут создаваться по соглашению при обнаружении связи, но в классе зависимой сущности не найдено свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="2d1c6-111">В этом случае будет введено свойство теневого внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="2d1c6-112">Свойство теневого внешнего ключа будет называться `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которая указывает на основную сущность, используется для именования).</span><span class="sxs-lookup"><span data-stu-id="2d1c6-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="2d1c6-113">Если имя свойства ключа участника содержит имя свойства навигации, то имя будет просто `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="2d1c6-114">Если в зависимой сущности нет свойства навигации, в его месте используется имя типа участника.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="2d1c6-115">Например, приведенный ниже листинг кода приведет к тому, что в сущность `Post` будет введено свойство `BlogId` Shadow.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a><span data-ttu-id="2d1c6-116">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="2d1c6-116">Data Annotations</span></span>

<span data-ttu-id="2d1c6-117">Теневые свойства не могут быть созданы с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2d1c6-118">Текучий API</span><span class="sxs-lookup"><span data-stu-id="2d1c6-118">Fluent API</span></span>

<span data-ttu-id="2d1c6-119">Для настройки теневых свойств можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="2d1c6-120">После вызова перегрузки строки `Property` можно связать любой из вызовов конфигурации для других свойств.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="2d1c6-121">Если имя, передаваемое в метод `Property`, совпадает с именем существующего свойства (теневого или определенного для класса сущностей), то код настроит это существующее свойство, а не создаст новое свойство Shadow.</span><span class="sxs-lookup"><span data-stu-id="2d1c6-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
