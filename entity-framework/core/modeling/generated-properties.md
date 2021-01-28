---
title: Сформированные значения — EF Core
description: Настройка создания значений для свойств при использовании Entity Framework Core
author: AndriySvyryd
ms.date: 1/10/2021
uid: core/modeling/generated-properties
ms.openlocfilehash: 76fa4454c88a5ef7afb9864c2a4b1063ac75e37e
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983551"
---
# <a name="generated-values"></a><span data-ttu-id="82ac8-103">Созданные значения</span><span class="sxs-lookup"><span data-stu-id="82ac8-103">Generated Values</span></span>

<span data-ttu-id="82ac8-104">Значения столбцов базы данных могут формироваться различными способами: первичные ключевые столбцы часто автоматически увеличивают целые числа, другие столбцы — по умолчанию или вычисленные значения и т. д. На этой странице подробно описаны различные шаблоны создания значений конфигурации с EF Core.</span><span class="sxs-lookup"><span data-stu-id="82ac8-104">Database columns can have their values generated in various ways: primary key columns are frequently auto-incrementing integers, other columns have default or computed values, etc. This page details various patterns for configuration value generation with EF Core.</span></span>

## <a name="default-values"></a><span data-ttu-id="82ac8-105">Значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="82ac8-105">Default values</span></span>

<span data-ttu-id="82ac8-106">В реляционных базах данных столбец можно настроить со значением по умолчанию. Если строка вставляется без значения для этого столбца, будет использоваться значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="82ac8-106">On relational databases, a column can be configured with a default value; if a row is inserted without a value for that column, the default value will be used.</span></span>

<span data-ttu-id="82ac8-107">Можно настроить значение по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="82ac8-107">You can configure a default value on a property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

<span data-ttu-id="82ac8-108">Можно также указать фрагмент SQL, используемый для вычисления значения по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="82ac8-108">You can also specify a SQL fragment that is used to calculate the default value:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

## <a name="computed-columns"></a><span data-ttu-id="82ac8-109">Вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="82ac8-109">Computed columns</span></span>

<span data-ttu-id="82ac8-110">В большинстве реляционных баз данных столбец можно настроить так, чтобы его значение было вычислено в базе данных, как правило, с выражением, ссылающимся на другие столбцы:</span><span class="sxs-lookup"><span data-stu-id="82ac8-110">On most relational databases, a column can be configured to have its value computed in the database, typically with an expression referring to other columns:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=DefaultComputedColumn&highlight=3)]

<span data-ttu-id="82ac8-111">В приведенном выше примере создается *виртуальный* вычисляемый столбец, значение которого вычисляются при каждом выборке из базы данных.</span><span class="sxs-lookup"><span data-stu-id="82ac8-111">The above creates a *virtual* computed column, whose value is computed every time it is fetched from the database.</span></span> <span data-ttu-id="82ac8-112">Можно также указать, что вычисляемый столбец должен *храниться* (иногда называемый *материализованным*), что означает, что он будет вычислен при каждом обновлении строки и хранится на диске вместе с обычными столбцами:</span><span class="sxs-lookup"><span data-stu-id="82ac8-112">You may also specify that a computed column be *stored* (sometimes called *persisted*), meaning that it is computed on every update of the row, and is stored on disk alongside regular columns:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=StoredComputedColumn&highlight=3)]

> [!NOTE]
> <span data-ttu-id="82ac8-113">В EF Core 5,0 добавлена поддержка создания сохраненных вычислений столбцов.</span><span class="sxs-lookup"><span data-stu-id="82ac8-113">Support for creating stored computed columns was added in EF Core 5.0.</span></span>

## <a name="primary-keys"></a><span data-ttu-id="82ac8-114">Первичные ключи</span><span class="sxs-lookup"><span data-stu-id="82ac8-114">Primary keys</span></span>

<span data-ttu-id="82ac8-115">В соответствии с соглашением Несоставные первичные ключи типа Short, int, long или GUID настраиваются так, чтобы иметь значения, формируемые для вставленных сущностей, если оно не предоставлено приложением.</span><span class="sxs-lookup"><span data-stu-id="82ac8-115">By convention, non-composite primary keys of type short, int, long, or Guid are set up to have values generated for inserted entities if a value isn't provided by the application.</span></span> <span data-ttu-id="82ac8-116">Поставщик базы данных обычно выполняет необходимую настройку. Например, числовой первичный ключ в SQL Server автоматически настраивается как столбец ИДЕНТИФИКАТОРов.</span><span class="sxs-lookup"><span data-stu-id="82ac8-116">Your database provider typically takes care of the necessary configuration; for example, a numeric primary key in SQL Server is automatically set up to be an IDENTITY column.</span></span>

<span data-ttu-id="82ac8-117">Дополнительные сведения см. [в документации по ключам](xref:core/modeling/keys).</span><span class="sxs-lookup"><span data-stu-id="82ac8-117">For more information, [see the documentation about keys](xref:core/modeling/keys).</span></span>

## <a name="explicitly-configuring-value-generation"></a><span data-ttu-id="82ac8-118">Явная настройка создания значений</span><span class="sxs-lookup"><span data-stu-id="82ac8-118">Explicitly configuring value generation</span></span>

<span data-ttu-id="82ac8-119">Мы узнали, что EF Core автоматически настраивает создание значений для первичных ключей, но может потребоваться сделать то же самое для неключевых свойств.</span><span class="sxs-lookup"><span data-stu-id="82ac8-119">We saw above that EF Core automatically sets up value generation for primary keys - but we may want to do the same for non-key properties.</span></span> <span data-ttu-id="82ac8-120">Можно настроить любое свойство так, чтобы его значение создавалось для вставленных сущностей следующим образом:</span><span class="sxs-lookup"><span data-stu-id="82ac8-120">You can configure any property to have its value generated for inserted entities as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="82ac8-121">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="82ac8-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-api"></a>[<span data-ttu-id="82ac8-122">Текучий API</span><span class="sxs-lookup"><span data-stu-id="82ac8-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

<span data-ttu-id="82ac8-123">Аналогично, для свойства можно настроить создание значения при добавлении или обновлении:</span><span class="sxs-lookup"><span data-stu-id="82ac8-123">Similarly, a property can be configured to have its value generated on add or update:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="82ac8-124">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="82ac8-124">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-api"></a>[<span data-ttu-id="82ac8-125">Текучий API</span><span class="sxs-lookup"><span data-stu-id="82ac8-125">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

<span data-ttu-id="82ac8-126">\*\*_</span><span class="sxs-lookup"><span data-stu-id="82ac8-126">\*\*_</span></span>

> [!WARNING]
> <span data-ttu-id="82ac8-127">В отличие от значений по умолчанию или от вычисленных столбцов, не указывается _how \* значения, которые должны быть созданы. Это зависит от используемого поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="82ac8-127">Unlike with default values or computed columns, we are not specifying _how\* the values are to be generated; that depends on the database provider being used.</span></span> <span data-ttu-id="82ac8-128">Поставщики баз данных могут автоматически настроить создание значений для некоторых типов свойств, но для других может потребоваться вручную настроить способ создания значения.</span><span class="sxs-lookup"><span data-stu-id="82ac8-128">Database providers may automatically set up value generation for some property types, but others may require you to manually set up how the value is generated.</span></span>
>
> <span data-ttu-id="82ac8-129">Например, на SQL Server, когда свойство GUID настроено в качестве значения, созданного при добавлении, поставщик автоматически выполняет клиентское создание значений, используя алгоритм для создания оптимальных последовательных значений GUID.</span><span class="sxs-lookup"><span data-stu-id="82ac8-129">For example, on SQL Server, when a GUID property is configured as value generated on add, the provider automatically performs value generation client-side, using an algorithm to generate optimal sequential GUID values.</span></span> <span data-ttu-id="82ac8-130">Однако указание `ValueGeneratedOnAdd()` свойства DateTime не будет действовать ([см. раздел ниже для создания значения DateTime](#datetime-value-generation)).</span><span class="sxs-lookup"><span data-stu-id="82ac8-130">However, specifying `ValueGeneratedOnAdd()` on a DateTime property will have no effect ([see the section below for DateTime value generation](#datetime-value-generation)).</span></span>
>
> <span data-ttu-id="82ac8-131">Аналогичным образом, свойства Byte [], настроенные как созданные при добавлении или обновлении и помеченные как маркеры параллелизма, настраиваются с типом данных rowversion, чтобы значения автоматически создавались в базе данных.</span><span class="sxs-lookup"><span data-stu-id="82ac8-131">Similarly, byte[] properties that are configured as generated on add or update and marked as concurrency tokens are set up with the rowversion data type, so that values are automatically generated in the database.</span></span> <span data-ttu-id="82ac8-132">Однако `ValueGeneratedOnAddOrUpdate()` Повторная настройка не будет действовать.</span><span class="sxs-lookup"><span data-stu-id="82ac8-132">However, specifying `ValueGeneratedOnAddOrUpdate()` will again have no effect.</span></span>
>
> [!NOTE]
> <span data-ttu-id="82ac8-133">В зависимости от используемого поставщика базы данных значения могут создаваться клиентом на стороне EF или в базе данных.</span><span class="sxs-lookup"><span data-stu-id="82ac8-133">Depending on the database provider being used, values may be generated client side by EF or in the database.</span></span> <span data-ttu-id="82ac8-134">Если значение создается базой данных, то EF может присвоить временное значение при добавлении сущности в контекст. Это временное значение затем будет заменено формируемым базой данных значением во время `SaveChanges()` .</span><span class="sxs-lookup"><span data-stu-id="82ac8-134">If the value is generated by the database, then EF may assign a temporary value when you add the entity to the context; this temporary value will then be replaced by the database generated value during `SaveChanges()`.</span></span> <span data-ttu-id="82ac8-135">Дополнительные сведения см. [в документах по временным значениям](xref:core/change-tracking/explicit-tracking#temporary-values).</span><span class="sxs-lookup"><span data-stu-id="82ac8-135">For more information, [see the docs on temporary values](xref:core/change-tracking/explicit-tracking#temporary-values).</span></span>

## <a name="datetime-value-generation"></a><span data-ttu-id="82ac8-136">Создание значения даты и времени</span><span class="sxs-lookup"><span data-stu-id="82ac8-136">Date/time value generation</span></span>

<span data-ttu-id="82ac8-137">Распространенным запросом является наличие столбца базы данных, содержащего дату и время первого добавления столбца (значение, созданное при добавлении), или Дата последнего обновления (значение, созданное при добавлении или обновлении).</span><span class="sxs-lookup"><span data-stu-id="82ac8-137">A common request is to have a database column which contains the date/time for when the column was first inserted (value generated on add), or for when it was last updated (value generated on add or update).</span></span> <span data-ttu-id="82ac8-138">Так как для этого существуют различные стратегии, поставщики EF Core обычно не настраивают автоматическое создание значений для столбцов даты и времени. это необходимо сделать самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="82ac8-138">As there are various strategies to do this, EF Core providers usually don't set up value generation automatically for date/time columns - you have to configure this yourself.</span></span>

### <a name="creation-timestamp"></a><span data-ttu-id="82ac8-139">Метка времени создания</span><span class="sxs-lookup"><span data-stu-id="82ac8-139">Creation timestamp</span></span>

<span data-ttu-id="82ac8-140">Настройка столбца даты и времени на создание метки времени создания строки обычно зависит от настройки значения по умолчанию с помощью соответствующей функции SQL.</span><span class="sxs-lookup"><span data-stu-id="82ac8-140">Configuring a date/time column to have the creation timestamp of the row is usually a matter of configuring a default value with the appropriate SQL function.</span></span> <span data-ttu-id="82ac8-141">Например, на SQL Server можно использовать следующее:</span><span class="sxs-lookup"><span data-stu-id="82ac8-141">For example, on SQL Server you may use the following:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

<span data-ttu-id="82ac8-142">Убедитесь, что выбрана соответствующая функция, так как может существовать несколько (например, `GETDATE()` VS `GETUTCDATE()` ).</span><span class="sxs-lookup"><span data-stu-id="82ac8-142">Be sure to select the appropriate function, as several may exist (e.g. `GETDATE()` vs. `GETUTCDATE()`).</span></span>

### <a name="update-timestamp"></a><span data-ttu-id="82ac8-143">Отметка времени обновления</span><span class="sxs-lookup"><span data-stu-id="82ac8-143">Update timestamp</span></span>

<span data-ttu-id="82ac8-144">Хотя сохраненные вычисляемые столбцы кажутся хорошим решением для управления отметками времени последнего обновления, базы данных обычно не позволяют указывать такие функции, как `GETDATE()` в вычисляемом столбце.</span><span class="sxs-lookup"><span data-stu-id="82ac8-144">Although stored computed columns seem like a good solution for managing last-updated timestamps, databases usually don't allow specifying functions such as `GETDATE()` in a computed column.</span></span> <span data-ttu-id="82ac8-145">В качестве альтернативы можно настроить триггер базы данных для достижения того же результата:</span><span class="sxs-lookup"><span data-stu-id="82ac8-145">As an alternative, you can set up a database trigger to achieve the same effect:</span></span>

```sql
CREATE TRIGGER [dbo].[Blogs_UPDATE] ON [dbo].[Blogs]
    AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;

    IF ((SELECT TRIGGER_NESTLEVEL()) > 1) RETURN;

    DECLARE @Id INT

    SELECT @Id = INSERTED.BlogId
    FROM INSERTED

    UPDATE dbo.Blogs
    SET LastUpdated = GETDATE()
    WHERE BlogId = @Id
END
```

<span data-ttu-id="82ac8-146">Сведения о создании триггеров см. в [документации по использованию необработанного SQL в миграции](xref:core/managing-schemas/migrations/managing#adding-raw-sql).</span><span class="sxs-lookup"><span data-stu-id="82ac8-146">For information on creating triggers, [see the documentation on using raw SQL in migrations](xref:core/managing-schemas/migrations/managing#adding-raw-sql).</span></span>

## <a name="overriding-value-generation"></a><span data-ttu-id="82ac8-147">Переопределение создания значения</span><span class="sxs-lookup"><span data-stu-id="82ac8-147">Overriding value generation</span></span>

<span data-ttu-id="82ac8-148">Несмотря на то, что свойство настроено для создания значений, во многих случаях вы можете явно указать его значение.</span><span class="sxs-lookup"><span data-stu-id="82ac8-148">Although a property is configured for value generation, in many cases you may still explicitly specify a value for it.</span></span> <span data-ttu-id="82ac8-149">Будет ли это работать, зависит от конкретного настроенного механизма создания значений. Хотя можно указать явное значение, а не использовать значение столбца по умолчанию, то то же самое невозможно сделать с вычисляемыми столбцами.</span><span class="sxs-lookup"><span data-stu-id="82ac8-149">Whether this will actually work depends on the specific value generation mechanism that has been configured; while you may specify an explicit value instead of using a column's default value, the same cannot be done with computed columns.</span></span>

<span data-ttu-id="82ac8-150">Чтобы переопределить создание значения с помощью явного значения, просто задайте для свойства любое значение, которое не является значением CLR по умолчанию для данного типа свойства (для, для, `null` `string` для и `0` `int` `Guid.Empty` `Guid` т. д.).</span><span class="sxs-lookup"><span data-stu-id="82ac8-150">To override value generation with an explicit value, simply set the property to any value that is not the CLR default value for that property's type (`null` for `string`, `0` for `int`, `Guid.Empty` for `Guid`, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="82ac8-151">Попытка вставки явных значений в SQL Server УДОСТОВЕРЕНие завершается сбоем по умолчанию; [Дополнительные сведения см. в этих документах](xref:core/providers/sql-server/value-generation#inserting-explicit-values-into-identity-columns).</span><span class="sxs-lookup"><span data-stu-id="82ac8-151">Trying to insert explicit values into SQL Server IDENTITY fails by default; [see these docs for a workaround](xref:core/providers/sql-server/value-generation#inserting-explicit-values-into-identity-columns).</span></span>

<span data-ttu-id="82ac8-152">Чтобы предоставить явное значение для свойств, которые были настроены в качестве значения, созданного при добавлении или обновлении, необходимо также настроить свойство следующим образом:</span><span class="sxs-lookup"><span data-stu-id="82ac8-152">To provide an explicit value for properties that have been configured as value generated on add or update, you must also configure the property as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdateWithPropertySaveBehavior.cs?name=ValueGeneratedOnAddOrUpdateWithPropertySaveBehavior&highlight=5)]

## <a name="no-value-generation"></a><span data-ttu-id="82ac8-153">Без создания значения</span><span class="sxs-lookup"><span data-stu-id="82ac8-153">No value generation</span></span>

<span data-ttu-id="82ac8-154">В отличие от конкретных сценариев, описанных выше, для свойств обычно не настроено создание значений. Это означает, что приложение всегда должно предоставлять значение, которое должно быть сохранено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="82ac8-154">Apart from specific scenarios such as those described above, properties typically have no value generation configured; this means that it's up to the application to always supply a value to be saved to the database.</span></span> <span data-ttu-id="82ac8-155">Перед добавлением в контекст это значение должно быть назначено новым сущностям.</span><span class="sxs-lookup"><span data-stu-id="82ac8-155">This value must be assigned to new entities before they are added to the context.</span></span>

<span data-ttu-id="82ac8-156">Однако в некоторых случаях может потребоваться отключить создание значений, настроенное по соглашению.</span><span class="sxs-lookup"><span data-stu-id="82ac8-156">However, in some cases you may want to disable value generation that has been set up by convention.</span></span> <span data-ttu-id="82ac8-157">Например, первичный ключ типа int обычно неявно настраивается как создаваемый при добавлении значения (например, столбец идентификаторов в SQL Server).</span><span class="sxs-lookup"><span data-stu-id="82ac8-157">For example, a primary key of type int is usually implicitly configured as value-generated-on-add (e.g. identity column on SQL Server).</span></span> <span data-ttu-id="82ac8-158">Это можно отключить следующими средствами:</span><span class="sxs-lookup"><span data-stu-id="82ac8-158">You can disable this via the following:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="82ac8-159">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="82ac8-159">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-api"></a>[<span data-ttu-id="82ac8-160">Текучий API</span><span class="sxs-lookup"><span data-stu-id="82ac8-160">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
