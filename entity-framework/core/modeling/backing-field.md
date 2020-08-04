---
title: Резервные поля — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: e015c4f3fca767d25bee179c027813bd9fcf4c07
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526762"
---
# <a name="backing-fields"></a><span data-ttu-id="8738b-102">Резервные поля</span><span class="sxs-lookup"><span data-stu-id="8738b-102">Backing Fields</span></span>

<span data-ttu-id="8738b-103">Резервные поля позволяют EF читать и (или) записывать в поле, а не в свойство.</span><span class="sxs-lookup"><span data-stu-id="8738b-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="8738b-104">Это может быть полезно, если Инкапсуляция в классе используется для ограничения использования и/или улучшения семантики доступа к данным с помощью кода приложения, но значение должно быть считано из базы данных и (или) записано в нее без использования этих ограничений и улучшений.</span><span class="sxs-lookup"><span data-stu-id="8738b-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="8738b-105">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="8738b-105">Basic configuration</span></span>

<span data-ttu-id="8738b-106">В соответствии с соглашением следующие поля будут обнаружены как резервные поля для заданного свойства (в порядке приоритета).</span><span class="sxs-lookup"><span data-stu-id="8738b-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="8738b-107">В следующем примере `Url` свойство настроено для использования в `_url` качестве резервного поля:</span><span class="sxs-lookup"><span data-stu-id="8738b-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="8738b-108">Обратите внимание, что резервные поля обнаруживаются только для свойств, которые включены в модель.</span><span class="sxs-lookup"><span data-stu-id="8738b-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="8738b-109">Дополнительные сведения о том, какие свойства включены в модель, см. в разделе [включение & за исключением свойств](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="8738b-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="8738b-110">Резервные поля также можно настроить с помощью аннотации данных (доступно в Ефкоре 5,0) или API Fluent, например если имя поля не соответствует приведенным выше соглашениям:</span><span class="sxs-lookup"><span data-stu-id="8738b-110">You can also configure backing fields by using a Data Annotation (available in EFCore 5.0) or the Fluent API, e.g. if the field name doesn't correspond to the above conventions:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="8738b-111">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="8738b-111">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/BackingField.cs?name=BackingField&highlight=7)]

### <a name="fluent-api"></a>[<span data-ttu-id="8738b-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="8738b-112">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="8738b-113">Доступ к полям и свойствам</span><span class="sxs-lookup"><span data-stu-id="8738b-113">Field and property access</span></span>

<span data-ttu-id="8738b-114">По умолчанию EF всегда будет считывать и записывать данные в резервное поле (предполагая, что одно из них настроено правильно) и никогда не будет использовать свойство.</span><span class="sxs-lookup"><span data-stu-id="8738b-114">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="8738b-115">Однако EF также поддерживает другие шаблоны доступа.</span><span class="sxs-lookup"><span data-stu-id="8738b-115">However, EF also supports other access patterns.</span></span> <span data-ttu-id="8738b-116">Например, следующий пример указывает EF на запись в резервное поле только при материализации, а также на использование свойства во всех остальных случаях:</span><span class="sxs-lookup"><span data-stu-id="8738b-116">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="8738b-117">Полный набор поддерживаемых параметров см. в [перечислении пропертякцессмоде](/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="8738b-117">See the [PropertyAccessMode enum](/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="8738b-118">В EF Core 3,0 режим доступа к свойству по умолчанию изменен с `PreferFieldDuringConstruction` на `PreferField` .</span><span class="sxs-lookup"><span data-stu-id="8738b-118">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="8738b-119">Свойства только для полей</span><span class="sxs-lookup"><span data-stu-id="8738b-119">Field-only properties</span></span>

<span data-ttu-id="8738b-120">Можно также создать концептуальное свойство в модели, которое не имеет соответствующего свойства CLR в классе сущности, а использует поле для хранения данных в сущности.</span><span class="sxs-lookup"><span data-stu-id="8738b-120">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="8738b-121">Это отличается от [теневых свойств](shadow-properties.md), где данные хранятся в средстве записи изменений, а не в типе CLR сущности.</span><span class="sxs-lookup"><span data-stu-id="8738b-121">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="8738b-122">Свойства, доступные только для полей, обычно используются, когда класс сущностей использует методы вместо свойств для получения и задания значений или в случаях, когда поля не должны быть представлены вообще в модели предметной области (например, первичные ключи).</span><span class="sxs-lookup"><span data-stu-id="8738b-122">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="8738b-123">Можно настроить свойство только для поля, указав имя в `Property(...)` API:</span><span class="sxs-lookup"><span data-stu-id="8738b-123">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="8738b-124">EF будет пытаться найти свойство CLR с заданным именем или поле, если свойство не найдено.</span><span class="sxs-lookup"><span data-stu-id="8738b-124">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="8738b-125">Если не найдено ни свойство, ни поле, будет настроено свойство теневого копирования.</span><span class="sxs-lookup"><span data-stu-id="8738b-125">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="8738b-126">Может потребоваться ссылка на свойство, доступное только для полей, из запросов LINQ, но такие поля обычно являются частными.</span><span class="sxs-lookup"><span data-stu-id="8738b-126">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="8738b-127">`EF.Property(...)`Метод в запросе LINQ можно использовать для ссылки на поле:</span><span class="sxs-lookup"><span data-stu-id="8738b-127">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
