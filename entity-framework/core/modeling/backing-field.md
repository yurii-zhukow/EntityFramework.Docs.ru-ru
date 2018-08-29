---
title: Резервные поля — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994097"
---
# <a name="backing-fields"></a><span data-ttu-id="cb10b-102">Резервные поля</span><span class="sxs-lookup"><span data-stu-id="cb10b-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="cb10b-103">Этот компонент впервые появился в EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="cb10b-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="cb10b-104">Резервные поля Разрешить EF на чтение или запись в поле, а не свойство.</span><span class="sxs-lookup"><span data-stu-id="cb10b-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="cb10b-105">Это может быть полезно, когда инкапсуляция в классе используется для ограничения использования и улучшения семантики вокруг доступ к данным в коде приложения, но значение следует чтения или записи в базу данных без использования этих ограничений / Улучшенные возможности.</span><span class="sxs-lookup"><span data-stu-id="cb10b-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="cb10b-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="cb10b-106">Conventions</span></span>

<span data-ttu-id="cb10b-107">По соглашению будут обнаружены следующие поля как резервные поля для заданного свойства (перечисленные в порядке приоритетности).</span><span class="sxs-lookup"><span data-stu-id="cb10b-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="cb10b-108">Поля обнаруживаются только для свойств, которые включаются в модели.</span><span class="sxs-lookup"><span data-stu-id="cb10b-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="cb10b-109">Дополнительные сведения, на котором включены свойства в модели, см. в разделе [Включение и исключение свойств](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="cb10b-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="cb10b-110">При настройке резервное поле EF записи непосредственно к этому полю при материализации экземпляров сущности из базы данных (а не с помощью метода задания свойства).</span><span class="sxs-lookup"><span data-stu-id="cb10b-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="cb10b-111">Если EF должен считывать или записывать значения в других случаях, она будет по возможности используйте свойство.</span><span class="sxs-lookup"><span data-stu-id="cb10b-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="cb10b-112">Например если EF необходимо обновить значение для свойства, он если он был определен будет использовать метод задания свойства.</span><span class="sxs-lookup"><span data-stu-id="cb10b-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="cb10b-113">Если свойство доступно только для чтения, он запишет к полю.</span><span class="sxs-lookup"><span data-stu-id="cb10b-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cb10b-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="cb10b-114">Data Annotations</span></span>

<span data-ttu-id="cb10b-115">Резервные поля нельзя настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="cb10b-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cb10b-116">Текучий API</span><span class="sxs-lookup"><span data-stu-id="cb10b-116">Fluent API</span></span>

<span data-ttu-id="cb10b-117">Fluent API можно использовать для настройки резервное поле для свойства.</span><span class="sxs-lookup"><span data-stu-id="cb10b-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="cb10b-118">Управление, если используется поле</span><span class="sxs-lookup"><span data-stu-id="cb10b-118">Controlling when the field is used</span></span>

<span data-ttu-id="cb10b-119">Вы можете настроить, когда EF использует поле или свойство.</span><span class="sxs-lookup"><span data-stu-id="cb10b-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="cb10b-120">См. в разделе [PropertyAccessMode перечисления](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) поддерживаемые параметры.</span><span class="sxs-lookup"><span data-stu-id="cb10b-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="cb10b-121">Поля типа без свойства</span><span class="sxs-lookup"><span data-stu-id="cb10b-121">Fields without a property</span></span>

<span data-ttu-id="cb10b-122">Также можно создать свойство концептуальной модели, не имеет соответствующего свойства среды CLR в классе сущности, но вместо этого использует поле для хранения данных в сущности.</span><span class="sxs-lookup"><span data-stu-id="cb10b-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="cb10b-123">Это отличается от [замещения свойств](shadow-properties.md), где данные хранятся в объекте отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="cb10b-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="cb10b-124">Это обычно будет использоваться, если класс сущностей использует методы для получения и задания значений.</span><span class="sxs-lookup"><span data-stu-id="cb10b-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="cb10b-125">Можно предоставить EF имя поля в `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="cb10b-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="cb10b-126">Если отсутствует свойство с заданным именем, EF будет выглядеть для поля.</span><span class="sxs-lookup"><span data-stu-id="cb10b-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="cb10b-127">Можно также даю свойству имя, отличное от имени поля.</span><span class="sxs-lookup"><span data-stu-id="cb10b-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="cb10b-128">Это имя используется при создании модели, прежде всего он будет использоваться для имени столбца, который сопоставляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cb10b-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="cb10b-129">Если отсутствует свойство в классе сущности, можно использовать `EF.Property(...)` метода в запросе LINQ для ссылки на свойство, которое по существу является частью модели.</span><span class="sxs-lookup"><span data-stu-id="cb10b-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
