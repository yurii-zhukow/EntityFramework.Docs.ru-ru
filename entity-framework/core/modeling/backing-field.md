---
title: Резервные поля — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414641"
---
# <a name="backing-fields"></a><span data-ttu-id="8a31a-102">Резервные поля</span><span class="sxs-lookup"><span data-stu-id="8a31a-102">Backing Fields</span></span>

<span data-ttu-id="8a31a-103">Резервные поля позволяют EF читать и (или) записывать в поле, а не в свойство.</span><span class="sxs-lookup"><span data-stu-id="8a31a-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="8a31a-104">Это может быть полезно, если Инкапсуляция в классе используется для ограничения использования и/или улучшения семантики доступа к данным с помощью кода приложения, но значение должно быть считано из базы данных и (или) записано в нее без использования этих ограничений и улучшений.</span><span class="sxs-lookup"><span data-stu-id="8a31a-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="8a31a-105">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="8a31a-105">Basic configuration</span></span>

<span data-ttu-id="8a31a-106">В соответствии с соглашением следующие поля будут обнаружены как резервные поля для заданного свойства (в порядке приоритета).</span><span class="sxs-lookup"><span data-stu-id="8a31a-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="8a31a-107">В следующем примере свойство `Url` настроено так, чтобы `_url` в качестве резервного поля:</span><span class="sxs-lookup"><span data-stu-id="8a31a-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="8a31a-108">Обратите внимание, что резервные поля обнаруживаются только для свойств, которые включены в модель.</span><span class="sxs-lookup"><span data-stu-id="8a31a-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="8a31a-109">Дополнительные сведения о том, какие свойства включены в модель, см. в разделе [включение & за исключением свойств](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="8a31a-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="8a31a-110">Можно также явно настроить резервное копирование полей, например, если имя поля не соответствует приведенным выше соглашениям:</span><span class="sxs-lookup"><span data-stu-id="8a31a-110">You can also configure backing fields explicitly, e.g. if the field name doesn't correspond to the above conventions:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="8a31a-111">Доступ к полям и свойствам</span><span class="sxs-lookup"><span data-stu-id="8a31a-111">Field and property access</span></span>

<span data-ttu-id="8a31a-112">По умолчанию EF всегда будет считывать и записывать данные в резервное поле (предполагая, что одно из них настроено правильно) и никогда не будет использовать свойство.</span><span class="sxs-lookup"><span data-stu-id="8a31a-112">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="8a31a-113">Однако EF также поддерживает другие шаблоны доступа.</span><span class="sxs-lookup"><span data-stu-id="8a31a-113">However, EF also supports other access patterns.</span></span> <span data-ttu-id="8a31a-114">Например, следующий пример указывает EF на запись в резервное поле только при материализации, а также на использование свойства во всех остальных случаях:</span><span class="sxs-lookup"><span data-stu-id="8a31a-114">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="8a31a-115">Полный набор поддерживаемых параметров см. в [перечислении пропертякцессмоде](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="8a31a-115">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="8a31a-116">В EF Core 3,0 режим доступа к свойству по умолчанию изменился с `PreferFieldDuringConstruction` на `PreferField`.</span><span class="sxs-lookup"><span data-stu-id="8a31a-116">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="8a31a-117">Свойства только для полей</span><span class="sxs-lookup"><span data-stu-id="8a31a-117">Field-only properties</span></span>

<span data-ttu-id="8a31a-118">Можно также создать концептуальное свойство в модели, которое не имеет соответствующего свойства CLR в классе сущности, а использует поле для хранения данных в сущности.</span><span class="sxs-lookup"><span data-stu-id="8a31a-118">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="8a31a-119">Это отличается от [теневых свойств](shadow-properties.md), где данные хранятся в средстве записи изменений, а не в типе CLR сущности.</span><span class="sxs-lookup"><span data-stu-id="8a31a-119">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="8a31a-120">Свойства, доступные только для полей, обычно используются, когда класс сущностей использует методы вместо свойств для получения и задания значений или в случаях, когда поля не должны быть представлены вообще в модели предметной области (например, первичные ключи).</span><span class="sxs-lookup"><span data-stu-id="8a31a-120">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="8a31a-121">Можно настроить свойство только для поля, указав имя в `Property(...)` API:</span><span class="sxs-lookup"><span data-stu-id="8a31a-121">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="8a31a-122">EF будет пытаться найти свойство CLR с заданным именем или поле, если свойство не найдено.</span><span class="sxs-lookup"><span data-stu-id="8a31a-122">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="8a31a-123">Если не найдено ни свойство, ни поле, будет настроено свойство теневого копирования.</span><span class="sxs-lookup"><span data-stu-id="8a31a-123">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="8a31a-124">Может потребоваться ссылка на свойство, доступное только для полей, из запросов LINQ, но такие поля обычно являются частными.</span><span class="sxs-lookup"><span data-stu-id="8a31a-124">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="8a31a-125">Для ссылки на поле можно использовать метод `EF.Property(...)` в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="8a31a-125">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
