---
title: Обязательные и необязательные свойства — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655660"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="13e51-102">Обязательные и дополнительные свойства</span><span class="sxs-lookup"><span data-stu-id="13e51-102">Required and Optional Properties</span></span>

<span data-ttu-id="13e51-103">Свойство считается необязательным, если оно является допустимым для того, чтобы оно содержало `null`.</span><span class="sxs-lookup"><span data-stu-id="13e51-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="13e51-104">Если `null` не является допустимым значением для присвоения свойству, оно считается обязательным свойством.</span><span class="sxs-lookup"><span data-stu-id="13e51-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="13e51-105">При сопоставлении со схемой реляционной базы данных обязательные свойства создаются как столбцы, не допускающие значения NULL, а дополнительные свойства создаются как столбцы, допускающие значение null.</span><span class="sxs-lookup"><span data-stu-id="13e51-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="13e51-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="13e51-106">Conventions</span></span>

<span data-ttu-id="13e51-107">По соглашению свойство, тип .NET которого может содержать значение null, будет настроено как необязательный, тогда как свойства, типы .NET которых не могут содержать значение null, будут настроены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="13e51-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="13e51-108">Например, все свойства с типами значений .NET (`int`, `decimal`, `bool`и т. д.) настраиваются как обязательные, а все свойства с типами значений .NET, допускающими значение null (`int?`, `decimal?`, `bool?`и т. д.), настраиваются как необязательные.</span><span class="sxs-lookup"><span data-stu-id="13e51-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="13e51-109">C#в 8 появилась новая функция, называемая [ссылочными типами, допускающими значение NULL](/dotnet/csharp/tutorials/nullable-reference-types), которая позволяет создавать заметки для ссылочных типов, указывающих, является ли он допустимым для того, чтобы они содержали значение null.</span><span class="sxs-lookup"><span data-stu-id="13e51-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="13e51-110">Эта функция отключена по умолчанию и, если она включена, изменяет поведение EF Core следующим образом:</span><span class="sxs-lookup"><span data-stu-id="13e51-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="13e51-111">Если ссылочные типы, допускающие значение null, отключены (по умолчанию), все свойства с ссылочными типами .NET настраиваются как необязательные по соглашению (например, `string`).</span><span class="sxs-lookup"><span data-stu-id="13e51-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="13e51-112">Если ссылочные типы, допускающие значение null, включены, свойства будут C# настроены на основе допустимости значений NULL для их типа .net: `string?` будет настроена как необязательная, тогда как `string` будет настроена в соответствии с требованиями.</span><span class="sxs-lookup"><span data-stu-id="13e51-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="13e51-113">В следующем примере показан тип сущности с обязательными и необязательными свойствами с отключенной ссылкой, допускающей значение null (по умолчанию) и включенной.</span><span class="sxs-lookup"><span data-stu-id="13e51-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="13e51-114">Без ссылочных типов, допускающих значения NULL (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="13e51-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="13e51-115">С ссылочными типами, допускающими значение null</span><span class="sxs-lookup"><span data-stu-id="13e51-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="13e51-116">Рекомендуется использовать ссылочные типы, допускающие значение null, так как она C# передает возможность принимать нулевое значение, выраженную в коде, в модель EF Core и в базу данных, а также устраняет использование заметок к данным в интерфейсе API или аннотациях данных для выражения одной концепции дважды</span><span class="sxs-lookup"><span data-stu-id="13e51-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="13e51-117">Соблюдайте осторожность при включении ссылочных типов, допускающих значение null, в существующем проекте: свойства ссылочного типа, которые ранее были настроены как необязательные, теперь будут настроены как обязательные, если только они не имеют явно заметку null.</span><span class="sxs-lookup"><span data-stu-id="13e51-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="13e51-118">При управлении схемой реляционной базы данных это может привести к созданию миграции, которые изменяют допустимость значений NULL в столбце базы данных.</span><span class="sxs-lookup"><span data-stu-id="13e51-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="13e51-119">Дополнительные сведения о ссылочных типах, допускающих значения NULL, и об их использовании с EF Core [см. на специальной странице документации по этой функции](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="13e51-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="13e51-120">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="13e51-120">Configuration</span></span>

<span data-ttu-id="13e51-121">Свойство, которое было бы необязательным по соглашению, может быть настроено следующим образом:</span><span class="sxs-lookup"><span data-stu-id="13e51-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="13e51-122">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="13e51-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="13e51-123">API Fluent</span><span class="sxs-lookup"><span data-stu-id="13e51-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
