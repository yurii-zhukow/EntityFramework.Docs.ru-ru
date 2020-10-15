---
title: Свойства сущности — EF Core
description: Настройка и сопоставьте свойства сущности с помощью Entity Framework Core
author: roji
ms.date: 05/27/2020
uid: core/modeling/entity-properties
ms.openlocfilehash: 99b0a9ee1e11714e98ebea8b2e6ac53bd2dc6e55
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062299"
---
# <a name="entity-properties"></a><span data-ttu-id="86a9b-103">Свойства сущности</span><span class="sxs-lookup"><span data-stu-id="86a9b-103">Entity Properties</span></span>

<span data-ttu-id="86a9b-104">Каждый тип сущности в модели имеет набор свойств, которые EF Core будут считывать и записывать данные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="86a9b-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="86a9b-105">Если используется реляционная база данных, то свойства сущности сопоставляются со столбцами таблицы.</span><span class="sxs-lookup"><span data-stu-id="86a9b-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="86a9b-106">Включенные и исключенные свойства</span><span class="sxs-lookup"><span data-stu-id="86a9b-106">Included and excluded properties</span></span>

<span data-ttu-id="86a9b-107">По соглашению все открытые свойства с методом считывания и методом задания будут включаться в модель.</span><span class="sxs-lookup"><span data-stu-id="86a9b-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="86a9b-108">Конкретные свойства можно исключить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="86a9b-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="86a9b-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86a9b-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[<span data-ttu-id="86a9b-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86a9b-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="86a9b-111">Имена столбцов</span><span class="sxs-lookup"><span data-stu-id="86a9b-111">Column names</span></span>

<span data-ttu-id="86a9b-112">По соглашению при использовании реляционной базы данных свойства сущности сопоставляются со столбцами таблицы, имеющими то же имя, что и свойство.</span><span class="sxs-lookup"><span data-stu-id="86a9b-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="86a9b-113">Если вы предпочитаете настраивать столбцы с разными именами, это можно сделать с помощью следующего фрагмента кода:</span><span class="sxs-lookup"><span data-stu-id="86a9b-113">If you prefer to configure your columns with different names, you can do so as following code snippet:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="86a9b-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86a9b-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[<span data-ttu-id="86a9b-115">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86a9b-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="86a9b-116">Типы данных столбцов</span><span class="sxs-lookup"><span data-stu-id="86a9b-116">Column data types</span></span>

<span data-ttu-id="86a9b-117">При использовании реляционной базы данных поставщик базы данных выбирает тип данных, основанный на типе .NET свойства.</span><span class="sxs-lookup"><span data-stu-id="86a9b-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="86a9b-118">Он также учитывает другие метаданные, такие как настроенная [Максимальная длина](#maximum-length), является ли свойство частью первичного ключа и т. д.</span><span class="sxs-lookup"><span data-stu-id="86a9b-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="86a9b-119">Например, SQL Server сопоставляет `DateTime` свойства со `datetime2(7)` столбцами, а `string` свойства — `nvarchar(max)` со столбцами (или для `nvarchar(450)` свойств, которые используются в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="86a9b-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="86a9b-120">Можно также настроить столбцы, указав для столбца точный тип данных.</span><span class="sxs-lookup"><span data-stu-id="86a9b-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="86a9b-121">Например, следующий код настраивает в `Url` качестве строки в формате, отличном от Юникод, максимальную длину `200` и `Rating` в десятичном формате с точностью `5` и масштабом `2` :</span><span class="sxs-lookup"><span data-stu-id="86a9b-121">For example, the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="86a9b-122">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86a9b-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[<span data-ttu-id="86a9b-123">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86a9b-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="86a9b-124">Максимальная длина</span><span class="sxs-lookup"><span data-stu-id="86a9b-124">Maximum length</span></span>

<span data-ttu-id="86a9b-125">Настройка максимальной длины предоставляет указание поставщику базы данных о соответствующем типе данных столбца для выбора конкретного свойства.</span><span class="sxs-lookup"><span data-stu-id="86a9b-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="86a9b-126">Максимальная длина применяется только к типам данных массива, таким как `string` и `byte[]` .</span><span class="sxs-lookup"><span data-stu-id="86a9b-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="86a9b-127">Entity Framework не выполняет никаких проверок максимальной длины перед передачей данных поставщику.</span><span class="sxs-lookup"><span data-stu-id="86a9b-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="86a9b-128">Поставщик или хранилище данных можно проверить, если это уместно.</span><span class="sxs-lookup"><span data-stu-id="86a9b-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="86a9b-129">Например, если целевой объект SQL Server, то превышение максимальной длины приведет к возникновению исключения, так как тип данных базового столбца не позволит сохранить излишние данные.</span><span class="sxs-lookup"><span data-stu-id="86a9b-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="86a9b-130">В следующем примере Настройка максимальной длины 500 приведет к созданию столбца типа, `nvarchar(500)` который будет создан на SQL Server:</span><span class="sxs-lookup"><span data-stu-id="86a9b-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="86a9b-131">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86a9b-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="86a9b-132">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86a9b-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

### <a name="precision-and-scale"></a><span data-ttu-id="86a9b-133">Точность и масштаб</span><span class="sxs-lookup"><span data-stu-id="86a9b-133">Precision and Scale</span></span>

<span data-ttu-id="86a9b-134">Начиная с Ефкоре 5,0 можно использовать API Fluent, чтобы настроить точность и масштаб.</span><span class="sxs-lookup"><span data-stu-id="86a9b-134">Starting with EFCore 5.0, you can use fluent API to configure the precision and scale.</span></span> <span data-ttu-id="86a9b-135">Он сообщает поставщику базы данных, какой объем хранилища требуется для данного столбца.</span><span class="sxs-lookup"><span data-stu-id="86a9b-135">It tells the database provider how much storage is needed for a given column.</span></span> <span data-ttu-id="86a9b-136">Он применяется только к типам данных, где поставщик допускает точность и масштабирование в зависимости от того, как правило `decimal` , и `DateTime` .</span><span class="sxs-lookup"><span data-stu-id="86a9b-136">It only applies to data types where the provider allows the precision and scale to vary - usually `decimal` and `DateTime`.</span></span>

<span data-ttu-id="86a9b-137">Для `decimal` свойств точность определяет максимальное количество цифр, необходимое для выражения любого значения, которое будет содержать столбец, а масштабирование определяет максимальное число требуемых десятичных разрядов.</span><span class="sxs-lookup"><span data-stu-id="86a9b-137">For `decimal` properties, precision defines the maximum number of digits needed to express any value the column will contain, and scale defines the maximum number of decimal places needed.</span></span> <span data-ttu-id="86a9b-138">Для `DateTime` свойств точность определяет максимальное количество цифр, необходимое для выражения долей секунд, а масштабирование не используется.</span><span class="sxs-lookup"><span data-stu-id="86a9b-138">For `DateTime` properties, precision defines the maximum number of digits needed to express fractions of seconds, and scale is not used.</span></span>

> [!NOTE]
> <span data-ttu-id="86a9b-139">Entity Framework не выполняет никаких проверок точности или масштабирования перед передачей данных поставщику.</span><span class="sxs-lookup"><span data-stu-id="86a9b-139">Entity Framework does not do any validation of precision or scale before passing data to the provider.</span></span> <span data-ttu-id="86a9b-140">Поставщик или хранилище данных должны проверяться соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="86a9b-140">It is up to the provider or data store to validate as appropriate.</span></span> <span data-ttu-id="86a9b-141">Например, при нацеливании на SQL Server столбец типа данных `datetime` не позволяет задать точность, тогда как у `datetime2` одной из них может быть точность от 0 до 7 включительно.</span><span class="sxs-lookup"><span data-stu-id="86a9b-141">For example, when targeting SQL Server, a column of data type `datetime` does not allow the precision to be set, whereas a `datetime2` one can have precision between 0 and 7 inclusive.</span></span>

<span data-ttu-id="86a9b-142">В следующем примере `Score` для свойства с точностью 14 и масштабом 2 будет `decimal(14,2)` создаваться столбец типа, который будет создан на SQL Server, а `LastUpdated` для свойства с точностью 3 будет использоваться столбец типа `datetime2(3)` :</span><span class="sxs-lookup"><span data-stu-id="86a9b-142">In the following example, configuring the `Score` property to have precision 14 and scale 2 will cause a column of type `decimal(14,2)` to be created on SQL Server, and configuring the `LastUpdated` property to have precision 3 will cause a column of type `datetime2(3)`:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="86a9b-143">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86a9b-143">Data Annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="86a9b-144">В настоящее время невозможно использовать заметки к данным для настройки.</span><span class="sxs-lookup"><span data-stu-id="86a9b-144">Currently not possible to use data annotations to configure.</span></span>

#### <a name="fluent-api"></a>[<span data-ttu-id="86a9b-145">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86a9b-145">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/PrecisionAndScale.cs?name=PrecisionAndScale&highlight=3-9)]

> [!NOTE]
> <span data-ttu-id="86a9b-146">Масштаб никогда не определяется без предварительного определения точности, поэтому API-интерфейсом Fluent для определения масштаба является `HasPrecision(precision, scale)` .</span><span class="sxs-lookup"><span data-stu-id="86a9b-146">Scale is never defined without first defining precision, so the Fluent API for defining the scale is `HasPrecision(precision, scale)`.</span></span>

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="86a9b-147">Обязательные и необязательные свойства</span><span class="sxs-lookup"><span data-stu-id="86a9b-147">Required and optional properties</span></span>

<span data-ttu-id="86a9b-148">Свойство считается необязательным, если оно является допустимым для его хранения `null` .</span><span class="sxs-lookup"><span data-stu-id="86a9b-148">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="86a9b-149">Если `null` значение не является допустимым для присвоения свойству, оно считается обязательным свойством.</span><span class="sxs-lookup"><span data-stu-id="86a9b-149">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="86a9b-150">При сопоставлении со схемой реляционной базы данных обязательные свойства создаются как столбцы, не допускающие значения NULL, а дополнительные свойства создаются как столбцы, допускающие значение null.</span><span class="sxs-lookup"><span data-stu-id="86a9b-150">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="86a9b-151">Соглашения</span><span class="sxs-lookup"><span data-stu-id="86a9b-151">Conventions</span></span>

<span data-ttu-id="86a9b-152">По соглашению свойство, тип .NET которого может содержать значение null, будет настроено как необязательный, тогда как свойства, типы .NET которых не могут содержать значение null, будут настроены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="86a9b-152">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="86a9b-153">Например, все свойства с типами значений .NET ( `int` , `decimal` , `bool` и т. д.) настраиваются как обязательные, а все свойства с типами значений .NET, допускающими значение null ( `int?` , `decimal?` , `bool?` и т. д.), настраиваются как необязательные.</span><span class="sxs-lookup"><span data-stu-id="86a9b-153">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="86a9b-154">В C# 8 появилась новая функция, именуемая [ссылочными типами, допускающими значение NULL](/dotnet/csharp/tutorials/nullable-reference-types), которая позволяет создавать заметки для ссылочных типов, указывающих, является ли он допустимым для того, чтобы они содержали значение null.</span><span class="sxs-lookup"><span data-stu-id="86a9b-154">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="86a9b-155">Эта функция отключена по умолчанию и, если она включена, изменяет поведение EF Core следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86a9b-155">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="86a9b-156">Если ссылочные типы, допускающие значение null, отключены (по умолчанию), все свойства с ссылочными типами .NET настраиваются как необязательные по соглашению (например, `string` ).</span><span class="sxs-lookup"><span data-stu-id="86a9b-156">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (for example, `string`).</span></span>
* <span data-ttu-id="86a9b-157">Если ссылочные типы, допускающие значение null, включены, то свойства будут настроены на основе допустимости значений NULL в C# для их типа .NET: `string?` будут настроены как необязательные, но `string` будут настроены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="86a9b-157">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, but `string` will be configured as required.</span></span>

<span data-ttu-id="86a9b-158">В следующем примере показан тип сущности с обязательными и необязательными свойствами с отключенной ссылкой, допускающей значение null (по умолчанию) и включенной.</span><span class="sxs-lookup"><span data-stu-id="86a9b-158">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-default"></a>[<span data-ttu-id="86a9b-159">Без ссылочных типов, допускающих значения NULL (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="86a9b-159">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[<span data-ttu-id="86a9b-160">С ссылочными типами, допускающими значение null</span><span class="sxs-lookup"><span data-stu-id="86a9b-160">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="86a9b-161">Рекомендуется использовать ссылочные типы, допускающие значение null, так как она передает возможность принимать нулевое значение, выраженную в коде C#, в модель EF Core и в базу данных, а также устраняет использование заметок к данным в API-интерфейсе или аннотациях данных для выражения дважды.</span><span class="sxs-lookup"><span data-stu-id="86a9b-161">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="86a9b-162">Соблюдайте осторожность при включении ссылочных типов, допускающих значение null, в существующем проекте: свойства ссылочного типа, которые ранее были настроены как необязательные, теперь будут настроены как обязательные, если только они не имеют явно заметку null.</span><span class="sxs-lookup"><span data-stu-id="86a9b-162">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="86a9b-163">При управлении схемой реляционной базы данных это может привести к созданию миграции, которые изменяют допустимость значений NULL в столбце базы данных.</span><span class="sxs-lookup"><span data-stu-id="86a9b-163">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="86a9b-164">Дополнительные сведения о ссылочных типах, допускающих значения NULL, и об их использовании с EF Core [см. на специальной странице документации по этой функции](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="86a9b-164">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="86a9b-165">Явная конфигурация</span><span class="sxs-lookup"><span data-stu-id="86a9b-165">Explicit configuration</span></span>

<span data-ttu-id="86a9b-166">Свойство, которое было бы необязательным по соглашению, может быть настроено следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86a9b-166">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="86a9b-167">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86a9b-167">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="86a9b-168">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86a9b-168">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***

## <a name="column-collations"></a><span data-ttu-id="86a9b-169">Параметры сортировки столбцов</span><span class="sxs-lookup"><span data-stu-id="86a9b-169">Column collations</span></span>

> [!NOTE]
> <span data-ttu-id="86a9b-170">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="86a9b-170">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="86a9b-171">Параметры сортировки могут быть определены для текстовых столбцов, определяя, как они сравниваются и упорядочиваются.</span><span class="sxs-lookup"><span data-stu-id="86a9b-171">A collation can be defined on text columns, determining how they are compared and ordered.</span></span> <span data-ttu-id="86a9b-172">Например, следующий фрагмент кода настраивает SQL Server столбца, в котором регистр не учитывается:</span><span class="sxs-lookup"><span data-stu-id="86a9b-172">For example, the following code snippet configures a SQL Server column to be case-insensitive:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Collations/Program.cs?range=42-43)]

<span data-ttu-id="86a9b-173">Если всем столбцам в базе данных необходимо использовать определенные параметры сортировки, то вместо этого определите параметры сортировки на уровне базы данных.</span><span class="sxs-lookup"><span data-stu-id="86a9b-173">If all columns in a database need to use a certain collation, define the collation at the database level instead.</span></span>

<span data-ttu-id="86a9b-174">Общие сведения о EF Core поддержки параметров сортировки можно найти на [странице документации по параметрам сортировки](xref:core/miscellaneous/collations-and-case-sensitivity).</span><span class="sxs-lookup"><span data-stu-id="86a9b-174">General information about EF Core support for collations can be found in the [collation documentation page](xref:core/miscellaneous/collations-and-case-sensitivity).</span></span>
