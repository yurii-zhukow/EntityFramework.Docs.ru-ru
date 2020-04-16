---
title: Резервные поля - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: d6faf7c0a07c7b855c1ce402ae29e19c80b9d3b4
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434179"
---
# <a name="backing-fields"></a><span data-ttu-id="b0a17-102">Резервные поля</span><span class="sxs-lookup"><span data-stu-id="b0a17-102">Backing Fields</span></span>

<span data-ttu-id="b0a17-103">Резервные поля позволяют EF читать и/или писать в поле, а не в свойство.</span><span class="sxs-lookup"><span data-stu-id="b0a17-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="b0a17-104">Это может быть полезно, когда инкапсуляция в классе используется для ограничения использования и/или повышения семантики вокруг доступа к данным по коду приложения, но значение должно быть прочитано и/или записано в базу данных без использования этих ограничений/улучшений.</span><span class="sxs-lookup"><span data-stu-id="b0a17-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="b0a17-105">Базовая конфигурация</span><span class="sxs-lookup"><span data-stu-id="b0a17-105">Basic configuration</span></span>

<span data-ttu-id="b0a17-106">По конвенции следующие поля будут обнаружены в качестве резервных полей для данного свойства (перечисленного в порядке приоритета).</span><span class="sxs-lookup"><span data-stu-id="b0a17-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="b0a17-107">В следующем примере `Url` свойство настроено `_url` в качестве резервного поля:</span><span class="sxs-lookup"><span data-stu-id="b0a17-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="b0a17-108">Обратите внимание, что резервные поля обнаруживаются только для свойств, включенных в модель.</span><span class="sxs-lookup"><span data-stu-id="b0a17-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="b0a17-109">Для получения дополнительной информации о том, какие свойства включены в модель, см [&.](included-properties.md)</span><span class="sxs-lookup"><span data-stu-id="b0a17-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="b0a17-110">Вы также можете настроить резервные поля с помощью аннотации данных или API Fluent, например, если имя поля не соответствует вышеуказанным конвенциям:</span><span class="sxs-lookup"><span data-stu-id="b0a17-110">You can also configure backing fields by using a Data Annotation or the Fluent API, e.g. if the field name doesn't correspond to the above conventions:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="b0a17-111">Аннотации данных</span><span class="sxs-lookup"><span data-stu-id="b0a17-111">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/BackingField.cs?name=BackingField&highlight=7)]

### <a name="fluent-api"></a>[<span data-ttu-id="b0a17-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="b0a17-112">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="b0a17-113">Доступ к полю и собственности</span><span class="sxs-lookup"><span data-stu-id="b0a17-113">Field and property access</span></span>

<span data-ttu-id="b0a17-114">По умолчанию EF всегда будет читать и писать в резервное поле - если один из них был правильно настроен - и никогда не будет использовать свойство.</span><span class="sxs-lookup"><span data-stu-id="b0a17-114">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="b0a17-115">Тем не менее, EF также поддерживает другие шаблоны доступа.</span><span class="sxs-lookup"><span data-stu-id="b0a17-115">However, EF also supports other access patterns.</span></span> <span data-ttu-id="b0a17-116">Например, следующий пример поручает EF писать в резервное поле только при материализации и использовать свойство во всех других случаях:</span><span class="sxs-lookup"><span data-stu-id="b0a17-116">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="b0a17-117">Ознакомьтесь с [перечней PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) для получения полного набора поддерживаемых опций.</span><span class="sxs-lookup"><span data-stu-id="b0a17-117">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="b0a17-118">С EF Core 3.0 режим доступа `PreferFieldDuringConstruction` к `PreferField`свойству по умолчанию изменился с .</span><span class="sxs-lookup"><span data-stu-id="b0a17-118">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="b0a17-119">Свойства только для полей</span><span class="sxs-lookup"><span data-stu-id="b0a17-119">Field-only properties</span></span>

<span data-ttu-id="b0a17-120">Можно также создать концептуальное свойство в модели, которое не имеет соответствующего свойства CLR в классе сущности, но вместо этого использует поле для хранения данных в объекте.</span><span class="sxs-lookup"><span data-stu-id="b0a17-120">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="b0a17-121">Это отличается от [Shadow Properties,](shadow-properties.md)где данные хранятся в трекере изменений, а не в типе CLR сущности.</span><span class="sxs-lookup"><span data-stu-id="b0a17-121">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="b0a17-122">Свойства только по полевым и только используются, когда класс сущности использует методы вместо свойств для получения/установленных значений, или в тех случаях, когда поля не должны подвергаться вообще в модели домена (например, первичные ключи).</span><span class="sxs-lookup"><span data-stu-id="b0a17-122">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="b0a17-123">Вы можете настроить свойство только для поля, `Property(...)` предоставив имя в API:</span><span class="sxs-lookup"><span data-stu-id="b0a17-123">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="b0a17-124">EF попытается найти свойство CLR с заданным именем или поле, если свойство не найдено.</span><span class="sxs-lookup"><span data-stu-id="b0a17-124">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="b0a17-125">Если не найдено ни свойство, ни поле, вместо него будет настроено свойство теней.</span><span class="sxs-lookup"><span data-stu-id="b0a17-125">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="b0a17-126">Возможно, потребуется обратиться к свойству только для поля из запросов LIN, но такие поля, как правило, являются частными.</span><span class="sxs-lookup"><span data-stu-id="b0a17-126">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="b0a17-127">Этот `EF.Property(...)` метод можно использовать в запросе LIN' для обозначения поля:</span><span class="sxs-lookup"><span data-stu-id="b0a17-127">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
