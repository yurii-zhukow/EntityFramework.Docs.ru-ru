---
title: Резервного поля — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053464"
---
# <a name="backing-fields"></a><span data-ttu-id="4f263-102">Резервные поля</span><span class="sxs-lookup"><span data-stu-id="4f263-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="4f263-103">Этот компонент впервые появился в EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4f263-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="4f263-104">Резервные поля Разрешить EF чтения или записи в поле, а не свойство.</span><span class="sxs-lookup"><span data-stu-id="4f263-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="4f263-105">Это может быть полезно, когда инкапсуляция в классе используется ограничение использования и/или улучшения семантики вокруг доступ к данным с помощью кода приложения, но значение следует чтения или записи в базу данных без использования этих ограничений или улучшения.</span><span class="sxs-lookup"><span data-stu-id="4f263-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="4f263-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="4f263-106">Conventions</span></span>

<span data-ttu-id="4f263-107">По соглашению следующие поля будут обнаружены как резервного поля для заданного свойства (перечислены в порядке приоритета).</span><span class="sxs-lookup"><span data-stu-id="4f263-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="4f263-108">Поля обнаруживаются только для свойств, которые включены в модель.</span><span class="sxs-lookup"><span data-stu-id="4f263-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="4f263-109">Включены свойства в модели Дополнительные сведения см. в разделе [включая & исключение свойств](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="4f263-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="4f263-110">При настройке резервное поле EF записи непосредственно к этому полю при материализации экземпляров сущностей из базы данных (вместо использования метода задания свойства).</span><span class="sxs-lookup"><span data-stu-id="4f263-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="4f263-111">Если EF должен прочитать или записать значение в другое время, он будет по возможности используйте свойство.</span><span class="sxs-lookup"><span data-stu-id="4f263-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="4f263-112">Например если EF должен обновить значение свойства, воспользуется метод задания свойства, если он был определен.</span><span class="sxs-lookup"><span data-stu-id="4f263-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="4f263-113">Если свойство доступно только для чтения, затем используется значение поля.</span><span class="sxs-lookup"><span data-stu-id="4f263-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4f263-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="4f263-114">Data Annotations</span></span>

<span data-ttu-id="4f263-115">Резервного поля нельзя настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="4f263-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4f263-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4f263-116">Fluent API</span></span>

<span data-ttu-id="4f263-117">Fluent API можно использовать для настройки резервное поле для свойства.</span><span class="sxs-lookup"><span data-stu-id="4f263-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="4f263-118">Управление, если используется поле</span><span class="sxs-lookup"><span data-stu-id="4f263-118">Controlling when the field is used</span></span>

<span data-ttu-id="4f263-119">Можно настроить при EF использует поле или свойство.</span><span class="sxs-lookup"><span data-stu-id="4f263-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="4f263-120">В разделе [PropertyAccessMode перечисления](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) поддерживаемые параметры.</span><span class="sxs-lookup"><span data-stu-id="4f263-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="4f263-121">Поля без свойства</span><span class="sxs-lookup"><span data-stu-id="4f263-121">Fields without a property</span></span>

<span data-ttu-id="4f263-122">Можно также создать концептуального свойства в модели, не имеет соответствующее свойство CLR в класс сущностей, но использует поле для хранения данных в сущности.</span><span class="sxs-lookup"><span data-stu-id="4f263-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="4f263-123">Это поведение отличается от [замещения свойств](shadow-properties.md), где данные хранятся в объекте отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="4f263-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="4f263-124">Это обычно будет использоваться, если класс сущностей использует методы для получения или задания значения.</span><span class="sxs-lookup"><span data-stu-id="4f263-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="4f263-125">Можно предоставить EF имя поля в `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="4f263-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="4f263-126">Если нет свойства с заданным именем, EF будет выглядеть для поля.</span><span class="sxs-lookup"><span data-stu-id="4f263-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="4f263-127">Можно также предоставить имя, отличное от имени поля свойства.</span><span class="sxs-lookup"><span data-stu-id="4f263-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="4f263-128">Это имя используется при создании модели, прежде всего будет использоваться для имени столбца, сопоставленного в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4f263-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="4f263-129">Если нет свойства в класс сущностей, можно использовать `EF.Property(...)` метод в запросе LINQ, ссылка на свойство, по существу является частью модели.</span><span class="sxs-lookup"><span data-stu-id="4f263-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
