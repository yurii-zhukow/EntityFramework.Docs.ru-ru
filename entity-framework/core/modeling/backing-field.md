---
title: Резервные поля — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811876"
---
# <a name="backing-fields"></a><span data-ttu-id="43042-102">Резервные поля</span><span class="sxs-lookup"><span data-stu-id="43042-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="43042-103">Эта функция впервые реализована в EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="43042-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="43042-104">Резервные поля позволяют EF читать и (или) записывать в поле, а не в свойство.</span><span class="sxs-lookup"><span data-stu-id="43042-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="43042-105">Это может быть полезно, если Инкапсуляция в классе используется для ограничения использования и (или) расширения семантики доступа к данным с помощью кода приложения, но значение должно считываться из базы данных и (или) записано в нее без использования этих ограничений/ усовершенствования.</span><span class="sxs-lookup"><span data-stu-id="43042-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="43042-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="43042-106">Conventions</span></span>

<span data-ttu-id="43042-107">В соответствии с соглашением следующие поля будут обнаружены как резервные поля для заданного свойства (в порядке приоритета).</span><span class="sxs-lookup"><span data-stu-id="43042-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="43042-108">Поля обнаруживаются только для свойств, которые включены в модель.</span><span class="sxs-lookup"><span data-stu-id="43042-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="43042-109">Дополнительные сведения о том, какие свойства включены в модель, см. в разделе [включение & за исключением свойств](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="43042-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="43042-110">Если резервное поле настроено, EF будет записывать непосредственно в это поле при материализации экземпляров сущностей из базы данных (вместо использования метода задания свойств).</span><span class="sxs-lookup"><span data-stu-id="43042-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="43042-111">Если EF необходимо прочитать или записать значение в другое время, по возможности будет использоваться свойство.</span><span class="sxs-lookup"><span data-stu-id="43042-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="43042-112">Например, если EF необходимо обновить значение свойства, он будет использовать метод задания свойства, если он определен.</span><span class="sxs-lookup"><span data-stu-id="43042-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="43042-113">Если свойство доступно только для чтения, оно будет записывать в поле.</span><span class="sxs-lookup"><span data-stu-id="43042-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="43042-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="43042-114">Data Annotations</span></span>

<span data-ttu-id="43042-115">Резервные поля не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="43042-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="43042-116">Текучий API</span><span class="sxs-lookup"><span data-stu-id="43042-116">Fluent API</span></span>

<span data-ttu-id="43042-117">С помощью API Fluent можно настроить резервное поле для свойства.</span><span class="sxs-lookup"><span data-stu-id="43042-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="43042-118">Управление при использовании поля</span><span class="sxs-lookup"><span data-stu-id="43042-118">Controlling when the field is used</span></span>

<span data-ttu-id="43042-119">Можно настроить, когда EF использует поле или свойство.</span><span class="sxs-lookup"><span data-stu-id="43042-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="43042-120">Поддерживаемые параметры см. в описании [перечисления пропертякцессмоде](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="43042-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="43042-121">Поля без свойства</span><span class="sxs-lookup"><span data-stu-id="43042-121">Fields without a property</span></span>

<span data-ttu-id="43042-122">Можно также создать концептуальное свойство в модели, которое не имеет соответствующего свойства CLR в классе сущности, а использует поле для хранения данных в сущности.</span><span class="sxs-lookup"><span data-stu-id="43042-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="43042-123">Это отличается от [теневых свойств](shadow-properties.md), где данные хранятся в средстве записи изменений.</span><span class="sxs-lookup"><span data-stu-id="43042-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="43042-124">Обычно это используется, если класс сущностей использует методы для получения или задания значений.</span><span class="sxs-lookup"><span data-stu-id="43042-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="43042-125">EF можно указать имя поля в `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="43042-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="43042-126">Если свойство с заданным именем отсутствует, то EF будет искать поле.</span><span class="sxs-lookup"><span data-stu-id="43042-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="43042-127">Если в классе сущностей нет свойства, можно использовать метод `EF.Property(...)` в запросе LINQ для ссылки на свойство, которое концептуально является частью модели.</span><span class="sxs-lookup"><span data-stu-id="43042-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
