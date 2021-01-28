---
title: Преобразования значений — EF Core
description: Настройка преобразователей значений в модели Entity Framework Core
author: ajcvickers
ms.date: 01/16/2021
uid: core/modeling/value-conversions
ms.openlocfilehash: d9d3753c7f0b257a2109e4af1f587df913c15b44
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983447"
---
# <a name="value-conversions"></a><span data-ttu-id="2124c-103">Преобразования значений</span><span class="sxs-lookup"><span data-stu-id="2124c-103">Value Conversions</span></span>

<span data-ttu-id="2124c-104">Преобразователи значений позволяют преобразовывать значения свойств при чтении из базы данных или записи в нее.</span><span class="sxs-lookup"><span data-stu-id="2124c-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="2124c-105">Это преобразование может быть из одного значения другого типа (например, для шифрования строк) или из значения одного типа в значение другого типа (например, преобразование перечисляемых значений в строки базы данных и из них).</span><span class="sxs-lookup"><span data-stu-id="2124c-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

> [!TIP]
> <span data-ttu-id="2124c-106">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span><span class="sxs-lookup"><span data-stu-id="2124c-106">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span></span>

## <a name="overview"></a><span data-ttu-id="2124c-107">Обзор</span><span class="sxs-lookup"><span data-stu-id="2124c-107">Overview</span></span>

<span data-ttu-id="2124c-108">Преобразователи значений указываются в терминах `ModelClrType` и `ProviderClrType` .</span><span class="sxs-lookup"><span data-stu-id="2124c-108">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="2124c-109">Тип модели — это тип .NET свойства в типе сущности.</span><span class="sxs-lookup"><span data-stu-id="2124c-109">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="2124c-110">Тип поставщика — это тип .NET, понятный поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-110">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="2124c-111">Например, чтобы сохранить перечисления в виде строк в базе данных, тип модели является типом перечисления, а тип поставщика — `String` .</span><span class="sxs-lookup"><span data-stu-id="2124c-111">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="2124c-112">Эти два типа могут быть одинаковыми.</span><span class="sxs-lookup"><span data-stu-id="2124c-112">These two types can be the same.</span></span>

<span data-ttu-id="2124c-113">Преобразования определяются с помощью двух `Func` деревьев выражений: одно из `ModelClrType` в `ProviderClrType` , а другое — от `ProviderClrType` до `ModelClrType` .</span><span class="sxs-lookup"><span data-stu-id="2124c-113">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="2124c-114">Деревья выражений используются, чтобы их можно было скомпилировать в делегат доступа к базе данных для эффективного преобразования.</span><span class="sxs-lookup"><span data-stu-id="2124c-114">Expression trees are used so that they can be compiled into the database access delegate for efficient conversions.</span></span> <span data-ttu-id="2124c-115">Дерево выражения может содержать простой вызов метода преобразования для сложных преобразований.</span><span class="sxs-lookup"><span data-stu-id="2124c-115">The expression tree may contain a simple call to a conversion method for complex conversions.</span></span>

> [!NOTE]
> <span data-ttu-id="2124c-116">Для свойства, которое было настроено для преобразования значений, также может потребоваться указать <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> .</span><span class="sxs-lookup"><span data-stu-id="2124c-116">A property that has been configured for value conversion may also need to specify a <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601>.</span></span> <span data-ttu-id="2124c-117">Дополнительные сведения см. в примерах ниже и документации по [компараторам значений](xref:core/modeling/value-comparers) .</span><span class="sxs-lookup"><span data-stu-id="2124c-117">See the examples below, and the [Value Comparers](xref:core/modeling/value-comparers) documentation for more information.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="2124c-118">Настройка преобразователя значений</span><span class="sxs-lookup"><span data-stu-id="2124c-118">Configuring a value converter</span></span>

<span data-ttu-id="2124c-119">Преобразования значений настраиваются в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="2124c-119">Value conversions are configured in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="2124c-120">Например, рассмотрим перечисление и тип сущности, определенный как:</span><span class="sxs-lookup"><span data-stu-id="2124c-120">For example, consider an enum and entity type defined as:</span></span>

<!--
        public class Rider
        {
            public int Id { get; set; }
            public EquineBeast Mount { get; set; }
        }

        public enum EquineBeast
        {
            Donkey,
            Mule,
            Horse,
            Unicorn
        }
-->
[!code-csharp[BeastAndRider](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=BeastAndRider)]

<span data-ttu-id="2124c-121">Преобразования могут быть настроены в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> для хранения значений перечисления в виде строк, таких как "донкэй", "Муле" и т. д. в базе данных. необходимо просто предоставить одну функцию, преобразующую из в `ModelClrType` `ProviderClrType` , и другую для обратного преобразования:</span><span class="sxs-lookup"><span data-stu-id="2124c-121">Conversions can be configured in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> to store the enum values as strings such as "Donkey", "Mule", etc. in the database; you simply need to provide one function which converts from the `ModelClrType` to the `ProviderClrType`, and another for the opposite conversion:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                modelBuilder
                    .Entity<Rider>()
                    .Property(e => e.Mount)
                    .HasConversion(
                        v => v.ToString(),
                        v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
            }
-->
[!code-csharp[ExplicitConversion](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ExplicitConversion)]

> [!NOTE]
> <span data-ttu-id="2124c-122">`null`Значение никогда не будет передано преобразователю значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-122">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="2124c-123">Значение NULL в столбце базы данных всегда равно NULL в экземпляре сущности и наоборот.</span><span class="sxs-lookup"><span data-stu-id="2124c-123">A null in a database column is always a null in the entity instance, and vice-versa.</span></span> <span data-ttu-id="2124c-124">Это упрощает реализацию преобразований и позволяет им совместно использовать свойства, допускающие значения NULL и не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="2124c-124">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span> <span data-ttu-id="2124c-125">Дополнительные сведения см. в [статье о проблемах #13850 GitHub](https://github.com/dotnet/efcore/issues/13850) .</span><span class="sxs-lookup"><span data-stu-id="2124c-125">See [GitHub issue #13850](https://github.com/dotnet/efcore/issues/13850) for more information.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="2124c-126">Предварительно определенные преобразования</span><span class="sxs-lookup"><span data-stu-id="2124c-126">Pre-defined conversions</span></span>

<span data-ttu-id="2124c-127">EF Core содержит множество предварительно определенных преобразований, которые избегают необходимость ручной записи функций преобразования.</span><span class="sxs-lookup"><span data-stu-id="2124c-127">EF Core contains many pre-defined conversions that avoid the need to write conversion functions manually.</span></span> <span data-ttu-id="2124c-128">Вместо этого EF Core выберет преобразование для использования на основе типа свойства в модели и запрошенного типа поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-128">Instead, EF Core will pick the conversion to use based on the property type in the model and the requested database provider type.</span></span>

<span data-ttu-id="2124c-129">Например, перечисление в преобразования строк используется как пример выше, но EF Core на самом деле сделает это автоматически, если тип поставщика настроен так, как `string` с помощью универсального типа <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.HasConversion%2A> :</span><span class="sxs-lookup"><span data-stu-id="2124c-129">For example, enum to string conversions are used as an example above, but EF Core will actually do this automatically when the provider type is configured as `string` using the generic type of <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.HasConversion%2A>:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                modelBuilder
                    .Entity<Rider>()
                    .Property(e => e.Mount)
                    .HasConversion<string>();
            }
-->
[!code-csharp[ConversionByClrType](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByClrType)]

<span data-ttu-id="2124c-130">То же самое можно достичь, явно указав тип столбца базы данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-130">The same thing can be achieved by explicitly specifying the database column type.</span></span> <span data-ttu-id="2124c-131">Например, если тип сущности определен следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2124c-131">For example, if the entity type is defined like so:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="2124c-132">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="2124c-132">Data Annotations</span></span>](#tab/data-annotations)

<!--
        public class Rider2
        {
            public int Id { get; set; }

            [Column(TypeName = "nvarchar(24)")]
            public EquineBeast Mount { get; set; }
        }
-->
[!code-csharp[ConversionByDatabaseType](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByDatabaseType)]

### <a name="fluent-api"></a>[<span data-ttu-id="2124c-133">Текучий API</span><span class="sxs-lookup"><span data-stu-id="2124c-133">Fluent API</span></span>](#tab/fluent-api)

<!--
                modelBuilder
                    .Entity<Rider2>()
                    .Property(e => e.Mount)
                    .HasColumnType("nvarchar(24)");
-->
[!code-csharp[ConversionByDatabaseTypeFluent](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByDatabaseTypeFluent)]

***

<span data-ttu-id="2124c-134">Затем значения перечисления будут сохранены в базе данных в виде строк без дополнительной настройки в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> .</span><span class="sxs-lookup"><span data-stu-id="2124c-134">Then the enum values will be saved as strings in the database without any further configuration in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="2124c-135">Класс ValueConverter</span><span class="sxs-lookup"><span data-stu-id="2124c-135">The ValueConverter class</span></span>

<span data-ttu-id="2124c-136">Вызов <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.HasConversion%2A> , как показано выше, создаст <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ValueConverter%602> экземпляр и установит его в свойстве.</span><span class="sxs-lookup"><span data-stu-id="2124c-136">Calling <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.HasConversion%2A> as shown above will create a <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ValueConverter%602> instance and set it on the property.</span></span> <span data-ttu-id="2124c-137">`ValueConverter`Вместо этого можно создать явно.</span><span class="sxs-lookup"><span data-stu-id="2124c-137">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="2124c-138">Пример:</span><span class="sxs-lookup"><span data-stu-id="2124c-138">For example:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var converter = new ValueConverter<EquineBeast, string>(
                    v => v.ToString(),
                    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

                modelBuilder
                    .Entity<Rider>()
                    .Property(e => e.Mount)
                    .HasConversion(converter);
            }
-->
[!code-csharp[ConversionByConverterInstance](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByConverterInstance)]

<span data-ttu-id="2124c-139">Это может быть полезно, если несколько свойств используют одно и то же преобразование.</span><span class="sxs-lookup"><span data-stu-id="2124c-139">This can be useful when multiple properties use the same conversion.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="2124c-140">Встроенные преобразователи</span><span class="sxs-lookup"><span data-stu-id="2124c-140">Built-in converters</span></span>

<span data-ttu-id="2124c-141">Как упоминалось выше, EF Core поставляется с набором предварительно определенных <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ValueConverter%602> классов, найденных в <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion> пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="2124c-141">As mentioned above, EF Core ships with a set of pre-defined <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ValueConverter%602> classes, found in the <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion> namespace.</span></span> <span data-ttu-id="2124c-142">Во многих случаях EF выберет соответствующий встроенный преобразователь, основанный на типе свойства в модели, и тип, запрошенный в базе данных, как показано выше для перечислений.</span><span class="sxs-lookup"><span data-stu-id="2124c-142">In many cases EF will choose the appropriate built-in converter based on the type of the property in the model and the type requested in the database, as shown above for enums.</span></span> <span data-ttu-id="2124c-143">Например, использование `.HasConversion<int>()` в `bool` свойстве приводит к тому, что EF Core преобразует логические значения в числовые ноль и одно значение:</span><span class="sxs-lookup"><span data-stu-id="2124c-143">For example, using `.HasConversion<int>()` on a `bool` property will cause EF Core to convert bool values to numerical zero and one values:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                modelBuilder
                    .Entity<User>()
                    .Property(e => e.IsActive)
                    .HasConversion<int>();
            }
-->
[!code-csharp[ConversionByBuiltInBoolToInt](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByBuiltInBoolToInt)]

<span data-ttu-id="2124c-144">Это функционально то же самое, что и создание экземпляра встроенного <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToZeroOneConverter%601> и его явное задание:</span><span class="sxs-lookup"><span data-stu-id="2124c-144">This is functionally the same as creating an instance of the built-in <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToZeroOneConverter%601> and setting it explicitly:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var converter = new BoolToZeroOneConverter<int>();

                modelBuilder
                    .Entity<User>()
                    .Property(e => e.IsActive)
                    .HasConversion(converter);
            }
-->
[!code-csharp[ConversionByBuiltInBoolToIntExplicit](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByBuiltInBoolToIntExplicit)]

<span data-ttu-id="2124c-145">В следующей таблице перечислены распространенные предварительно определенные преобразования из типов модели или свойства в типы поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-145">The following table summarizes commonly-used pre-defined conversions from model/property types to database provider types.</span></span> <span data-ttu-id="2124c-146">В таблице `any_numeric_type` означает один из способов `int` :,,,, `short` `long` `byte` `uint` , `ushort` , `ulong` , `sbyte` ,,, `char` `decimal` `float` или `double` .</span><span class="sxs-lookup"><span data-stu-id="2124c-146">In the table `any_numeric_type` means one of `int`, `short`, `long`, `byte`, `uint`, `ushort`, `ulong`, `sbyte`, `char`, `decimal`, `float`, or `double`.</span></span>

| <span data-ttu-id="2124c-147">Тип модели или свойства</span><span class="sxs-lookup"><span data-stu-id="2124c-147">Model/property type</span></span> | <span data-ttu-id="2124c-148">Тип поставщика или базы данных</span><span class="sxs-lookup"><span data-stu-id="2124c-148">Provider/database type</span></span> | <span data-ttu-id="2124c-149">Преобразование</span><span class="sxs-lookup"><span data-stu-id="2124c-149">Conversion</span></span>                                                | <span data-ttu-id="2124c-150">Использование</span><span class="sxs-lookup"><span data-stu-id="2124c-150">Usage</span></span>
|:--------------------|------------------------|-----------------------------------------------------------|------
| <span data-ttu-id="2124c-151">bool</span><span class="sxs-lookup"><span data-stu-id="2124c-151">bool</span></span>                | <span data-ttu-id="2124c-152">any_numeric_type</span><span class="sxs-lookup"><span data-stu-id="2124c-152">any_numeric_type</span></span>       | <span data-ttu-id="2124c-153">False/true в 0/1</span><span class="sxs-lookup"><span data-stu-id="2124c-153">False/true to 0/1</span></span>                                         | `.HasConversion<any_numeric_type>()`
|                     | <span data-ttu-id="2124c-154">any_numeric_type</span><span class="sxs-lookup"><span data-stu-id="2124c-154">any_numeric_type</span></span>       | <span data-ttu-id="2124c-155">False/true для любого из двух чисел</span><span class="sxs-lookup"><span data-stu-id="2124c-155">False/true to any two numbers</span></span>                             | <span data-ttu-id="2124c-156">Используйте <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToTwoValuesConverter%601>.</span><span class="sxs-lookup"><span data-stu-id="2124c-156">Use <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToTwoValuesConverter%601></span></span>
|                     | <span data-ttu-id="2124c-157">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-157">string</span></span>                 | <span data-ttu-id="2124c-158">False/true для "Y"/"N"</span><span class="sxs-lookup"><span data-stu-id="2124c-158">False/true to "Y"/"N"</span></span>                                     | `.HasConversion<string>()`
|                     | <span data-ttu-id="2124c-159">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-159">string</span></span>                 | <span data-ttu-id="2124c-160">False/true для любых двух строк</span><span class="sxs-lookup"><span data-stu-id="2124c-160">False/true to any two strings</span></span>                             | <span data-ttu-id="2124c-161">Используйте <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToStringConverter>.</span><span class="sxs-lookup"><span data-stu-id="2124c-161">Use <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToStringConverter></span></span>
| <span data-ttu-id="2124c-162">any_numeric_type</span><span class="sxs-lookup"><span data-stu-id="2124c-162">any_numeric_type</span></span>    | <span data-ttu-id="2124c-163">bool</span><span class="sxs-lookup"><span data-stu-id="2124c-163">bool</span></span>                   | <span data-ttu-id="2124c-164">0/1 в значение false/true</span><span class="sxs-lookup"><span data-stu-id="2124c-164">0/1 to false/true</span></span>                                         | `.HasConversion<bool>()`
|                     | <span data-ttu-id="2124c-165">any_numeric_type</span><span class="sxs-lookup"><span data-stu-id="2124c-165">any_numeric_type</span></span>       | <span data-ttu-id="2124c-166">Простое приведение</span><span class="sxs-lookup"><span data-stu-id="2124c-166">Simple cast</span></span>                                               | `.HasConversion<any_numeric_type>()`
|                     | <span data-ttu-id="2124c-167">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-167">string</span></span>                 | <span data-ttu-id="2124c-168">Число в виде строки</span><span class="sxs-lookup"><span data-stu-id="2124c-168">The number as a string</span></span>                                    | `.HasConversion<string>()`
| <span data-ttu-id="2124c-169">Перечисление</span><span class="sxs-lookup"><span data-stu-id="2124c-169">Enum</span></span>                | <span data-ttu-id="2124c-170">any_numeric_type</span><span class="sxs-lookup"><span data-stu-id="2124c-170">any_numeric_type</span></span>       | <span data-ttu-id="2124c-171">Числовое значение перечисления</span><span class="sxs-lookup"><span data-stu-id="2124c-171">The numeric value of the enum</span></span>                             | `.HasConversion<any_numeric_type>()`
|                     | <span data-ttu-id="2124c-172">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-172">string</span></span>                 | <span data-ttu-id="2124c-173">Строковое представление значения перечисления</span><span class="sxs-lookup"><span data-stu-id="2124c-173">The string representation of the enum value</span></span>               | `.HasConversion<string>()`
| <span data-ttu-id="2124c-174">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-174">string</span></span>              | <span data-ttu-id="2124c-175">bool</span><span class="sxs-lookup"><span data-stu-id="2124c-175">bool</span></span>                   | <span data-ttu-id="2124c-176">Анализирует строку как логическое значение</span><span class="sxs-lookup"><span data-stu-id="2124c-176">Parses the string as a bool</span></span>                               | `.HasConversion<bool>()`
|                     | <span data-ttu-id="2124c-177">any_numeric_type</span><span class="sxs-lookup"><span data-stu-id="2124c-177">any_numeric_type</span></span>       | <span data-ttu-id="2124c-178">Анализирует строку как заданный числовой тип</span><span class="sxs-lookup"><span data-stu-id="2124c-178">Parses the string as the given numeric type</span></span>               | `.HasConversion<any_numeric_type>()`
|                     | <span data-ttu-id="2124c-179">char</span><span class="sxs-lookup"><span data-stu-id="2124c-179">char</span></span>                   | <span data-ttu-id="2124c-180">Первый символ строки</span><span class="sxs-lookup"><span data-stu-id="2124c-180">The first character of the string</span></span>                         | `.HasConversion<char>()`
|                     | <span data-ttu-id="2124c-181">Дата и время</span><span class="sxs-lookup"><span data-stu-id="2124c-181">DateTime</span></span>               | <span data-ttu-id="2124c-182">Анализирует строку как дату и время</span><span class="sxs-lookup"><span data-stu-id="2124c-182">Parses the string as a DateTime</span></span>                           | `.HasConversion<DateTime>()`
|                     | <span data-ttu-id="2124c-183">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="2124c-183">DateTimeOffset</span></span>         | <span data-ttu-id="2124c-184">Выполняет синтаксический анализ строки как DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="2124c-184">Parses the string as a DateTimeOffset</span></span>                     | `.HasConversion<DateTimeOffset>()`
|                     | <span data-ttu-id="2124c-185">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="2124c-185">TimeSpan</span></span>               | <span data-ttu-id="2124c-186">Анализирует строку как интервал времени</span><span class="sxs-lookup"><span data-stu-id="2124c-186">Parses the string as a TimeSpan</span></span>                           | `.HasConversion<TimeSpan>()`
|                     | <span data-ttu-id="2124c-187">Guid</span><span class="sxs-lookup"><span data-stu-id="2124c-187">Guid</span></span>                   | <span data-ttu-id="2124c-188">Анализирует строку как идентификатор GUID</span><span class="sxs-lookup"><span data-stu-id="2124c-188">Parses the string as a Guid</span></span>                               | `.HasConversion<Guid>()`
|                     | <span data-ttu-id="2124c-189">byte[]</span><span class="sxs-lookup"><span data-stu-id="2124c-189">byte[]</span></span>                 | <span data-ttu-id="2124c-190">Строка в формате UTF8 bytes</span><span class="sxs-lookup"><span data-stu-id="2124c-190">The string as UTF8 bytes</span></span>                                  | `.HasConversion<byte[]>()`
| <span data-ttu-id="2124c-191">char</span><span class="sxs-lookup"><span data-stu-id="2124c-191">char</span></span>                | <span data-ttu-id="2124c-192">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-192">string</span></span>                 | <span data-ttu-id="2124c-193">Одиночная символьная строка</span><span class="sxs-lookup"><span data-stu-id="2124c-193">A single character string</span></span>                                 | `.HasConversion<string>()`
| <span data-ttu-id="2124c-194">Дата и время</span><span class="sxs-lookup"><span data-stu-id="2124c-194">DateTime</span></span>            | <span data-ttu-id="2124c-195">long</span><span class="sxs-lookup"><span data-stu-id="2124c-195">long</span></span>                   | <span data-ttu-id="2124c-196">Кодированная Дата и время с сохранением DateTime. Kind</span><span class="sxs-lookup"><span data-stu-id="2124c-196">Encoded date/time preserving DateTime.Kind</span></span>                | `.HasConversion<long>()`
|                     | <span data-ttu-id="2124c-197">long</span><span class="sxs-lookup"><span data-stu-id="2124c-197">long</span></span>                   | <span data-ttu-id="2124c-198">Тактов</span><span class="sxs-lookup"><span data-stu-id="2124c-198">Ticks</span></span>                                                     | <span data-ttu-id="2124c-199">Используйте <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToTicksConverter>.</span><span class="sxs-lookup"><span data-stu-id="2124c-199">Use <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToTicksConverter></span></span>
|                     | <span data-ttu-id="2124c-200">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-200">string</span></span>                 | <span data-ttu-id="2124c-201">Инвариантная строка даты и времени языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="2124c-201">Invariant culture date/time string</span></span>                        | `.HasConversion<string>()`
| <span data-ttu-id="2124c-202">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="2124c-202">DateTimeOffset</span></span>      | <span data-ttu-id="2124c-203">long</span><span class="sxs-lookup"><span data-stu-id="2124c-203">long</span></span>                   | <span data-ttu-id="2124c-204">Закодированные Дата и время со смещением</span><span class="sxs-lookup"><span data-stu-id="2124c-204">Encoded date/time with offset</span></span>                             | `.HasConversion<long>()`
|                     | <span data-ttu-id="2124c-205">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-205">string</span></span>                 | <span data-ttu-id="2124c-206">Инвариантная строка даты и времени языка и региональных параметров со смещением</span><span class="sxs-lookup"><span data-stu-id="2124c-206">Invariant culture date/time string with offset</span></span>            | `.HasConversion<string>()`
| <span data-ttu-id="2124c-207">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="2124c-207">TimeSpan</span></span>            | <span data-ttu-id="2124c-208">long</span><span class="sxs-lookup"><span data-stu-id="2124c-208">long</span></span>                   | <span data-ttu-id="2124c-209">Тактов</span><span class="sxs-lookup"><span data-stu-id="2124c-209">Ticks</span></span>                                                     | `.HasConversion<long>()`
|                     | <span data-ttu-id="2124c-210">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-210">string</span></span>                 | <span data-ttu-id="2124c-211">Строка инвариантного периода времени культуры</span><span class="sxs-lookup"><span data-stu-id="2124c-211">Invariant culture time span string</span></span>                        | `.HasConversion<string>()`
| <span data-ttu-id="2124c-212">URI</span><span class="sxs-lookup"><span data-stu-id="2124c-212">Uri</span></span>                 | <span data-ttu-id="2124c-213">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-213">string</span></span>                 | <span data-ttu-id="2124c-214">Универсальный код ресурса (URI) в виде строки</span><span class="sxs-lookup"><span data-stu-id="2124c-214">The URI as a string</span></span>                                       | `.HasConversion<string>()`
| <span data-ttu-id="2124c-215">PhysicalAddress</span><span class="sxs-lookup"><span data-stu-id="2124c-215">PhysicalAddress</span></span>     | <span data-ttu-id="2124c-216">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-216">string</span></span>                 | <span data-ttu-id="2124c-217">Адрес в виде строки</span><span class="sxs-lookup"><span data-stu-id="2124c-217">The address as a string</span></span>                                   | `.HasConversion<string>()`
|                     | <span data-ttu-id="2124c-218">byte[]</span><span class="sxs-lookup"><span data-stu-id="2124c-218">byte[]</span></span>                 | <span data-ttu-id="2124c-219">Байт в порядке с обратным порядком байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-219">Bytes in big-endian network order</span></span>                         | `.HasConversion<byte[]>()`
| <span data-ttu-id="2124c-220">IPAddress</span><span class="sxs-lookup"><span data-stu-id="2124c-220">IPAddress</span></span>           | <span data-ttu-id="2124c-221">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-221">string</span></span>                 | <span data-ttu-id="2124c-222">Адрес в виде строки</span><span class="sxs-lookup"><span data-stu-id="2124c-222">The address as a string</span></span>                                   | `.HasConversion<string>()`
|                     | <span data-ttu-id="2124c-223">byte[]</span><span class="sxs-lookup"><span data-stu-id="2124c-223">byte[]</span></span>                 | <span data-ttu-id="2124c-224">Байт в порядке с обратным порядком байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-224">Bytes in big-endian network order</span></span>                         | `.HasConversion<byte[]>()`
| <span data-ttu-id="2124c-225">Guid</span><span class="sxs-lookup"><span data-stu-id="2124c-225">Guid</span></span>                | <span data-ttu-id="2124c-226">строка</span><span class="sxs-lookup"><span data-stu-id="2124c-226">string</span></span>                 | <span data-ttu-id="2124c-227">GUID в формате "dddddddd-dddd-dddd-dddd-dddddddddddd"</span><span class="sxs-lookup"><span data-stu-id="2124c-227">The GUID in 'dddddddd-dddd-dddd-dddd-dddddddddddd' format</span></span> | `.HasConversion<string>()`
|                     | <span data-ttu-id="2124c-228">byte[]</span><span class="sxs-lookup"><span data-stu-id="2124c-228">byte[]</span></span>                 | <span data-ttu-id="2124c-229">Байт в порядке двоичной сериализации .NET</span><span class="sxs-lookup"><span data-stu-id="2124c-229">Bytes in .NET binary serialization order</span></span>                  | `.HasConversion<byte[]>()`

<span data-ttu-id="2124c-230">Обратите внимание, что в этих преобразованиях предполагается, что формат значения подходит для преобразования.</span><span class="sxs-lookup"><span data-stu-id="2124c-230">Note that these conversions assume that the format of the value is appropriate for the conversion.</span></span> <span data-ttu-id="2124c-231">Например, преобразование строк в числа завершится ошибкой, если строковые значения не могут быть проанализированы как числа.</span><span class="sxs-lookup"><span data-stu-id="2124c-231">For example, converting strings to numbers will fail if the string values cannot be parsed as numbers.</span></span>

<span data-ttu-id="2124c-232">Полный список встроенных конвертеров:</span><span class="sxs-lookup"><span data-stu-id="2124c-232">The full list of built-in converters is:</span></span>

* <span data-ttu-id="2124c-233">Преобразование свойств bool:</span><span class="sxs-lookup"><span data-stu-id="2124c-233">Converting bool properties:</span></span>
  * <span data-ttu-id="2124c-234"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToStringConverter> -Bool для строк, таких как "Y" и "N"</span><span class="sxs-lookup"><span data-stu-id="2124c-234"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToStringConverter> - Bool to strings such as "Y" and "N"</span></span>
  * <span data-ttu-id="2124c-235"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToTwoValuesConverter%601> -Bool для любых двух значений</span><span class="sxs-lookup"><span data-stu-id="2124c-235"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToTwoValuesConverter%601> - Bool to any two values</span></span>
  * <span data-ttu-id="2124c-236"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToZeroOneConverter%601> -Bool — ноль и один</span><span class="sxs-lookup"><span data-stu-id="2124c-236"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToZeroOneConverter%601> - Bool to zero and one</span></span>
* <span data-ttu-id="2124c-237">Преобразование свойств массива байтов:</span><span class="sxs-lookup"><span data-stu-id="2124c-237">Converting byte array properties:</span></span>
  * <span data-ttu-id="2124c-238"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BytesToStringConverter> — Массив байтов в строку в кодировке Base64</span><span class="sxs-lookup"><span data-stu-id="2124c-238"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BytesToStringConverter> - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="2124c-239">Любое преобразование, для которого требуется только приведение типов</span><span class="sxs-lookup"><span data-stu-id="2124c-239">Any conversion that requires only a type-cast</span></span>
  * <span data-ttu-id="2124c-240"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.CastingConverter%602> — Преобразования, для которых требуется только приведение типов</span><span class="sxs-lookup"><span data-stu-id="2124c-240"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.CastingConverter%602> - Conversions that require only a type cast</span></span>
* <span data-ttu-id="2124c-241">Преобразование свойств char:</span><span class="sxs-lookup"><span data-stu-id="2124c-241">Converting char properties:</span></span>
  * <span data-ttu-id="2124c-242"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.CharToStringConverter> -Char — строка одиночного символа</span><span class="sxs-lookup"><span data-stu-id="2124c-242"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.CharToStringConverter> - Char to single character string</span></span>
* <span data-ttu-id="2124c-243">Преобразование <xref:System.DateTimeOffset> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-243">Converting <xref:System.DateTimeOffset> properties:</span></span>
  * <span data-ttu-id="2124c-244"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToBinaryConverter> - <xref:System.DateTimeOffset> в двоичное 64-разрядное значение</span><span class="sxs-lookup"><span data-stu-id="2124c-244"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToBinaryConverter> - <xref:System.DateTimeOffset> to binary-encoded 64-bit value</span></span>
  * <span data-ttu-id="2124c-245"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToBytesConverter> - <xref:System.DateTimeOffset> в массив байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-245"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToBytesConverter> - <xref:System.DateTimeOffset> to byte array</span></span>
  * <span data-ttu-id="2124c-246"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToStringConverter> - <xref:System.DateTimeOffset> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-246"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToStringConverter> - <xref:System.DateTimeOffset> to string</span></span>
* <span data-ttu-id="2124c-247">Преобразование <xref:System.DateTime> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-247">Converting <xref:System.DateTime> properties:</span></span>
  * <span data-ttu-id="2124c-248"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToBinaryConverter> - <xref:System.DateTime> в 64-разрядное значение, включая DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="2124c-248"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToBinaryConverter> - <xref:System.DateTime> to 64-bit value including DateTimeKind</span></span>
  * <span data-ttu-id="2124c-249"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToStringConverter> - <xref:System.DateTime> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-249"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToStringConverter> - <xref:System.DateTime> to string</span></span>
  * <span data-ttu-id="2124c-250"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToTicksConverter> - <xref:System.DateTime> в такты</span><span class="sxs-lookup"><span data-stu-id="2124c-250"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToTicksConverter> - <xref:System.DateTime> to ticks</span></span>
* <span data-ttu-id="2124c-251">Преобразование свойств перечисления:</span><span class="sxs-lookup"><span data-stu-id="2124c-251">Converting enum properties:</span></span>
  * <span data-ttu-id="2124c-252"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.EnumToNumberConverter%602> -Перечислить в базовое число</span><span class="sxs-lookup"><span data-stu-id="2124c-252"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.EnumToNumberConverter%602> - Enum to underlying number</span></span>
  * <span data-ttu-id="2124c-253"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.EnumToStringConverter%601> -Enum в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-253"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.EnumToStringConverter%601> - Enum to string</span></span>
* <span data-ttu-id="2124c-254">Преобразование <xref:System.Guid> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-254">Converting <xref:System.Guid> properties:</span></span>
  * <span data-ttu-id="2124c-255"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.GuidToBytesConverter> - <xref:System.Guid> в массив байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-255"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.GuidToBytesConverter> - <xref:System.Guid> to byte array</span></span>
  * <span data-ttu-id="2124c-256"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.GuidToStringConverter> - <xref:System.Guid> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-256"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.GuidToStringConverter> - <xref:System.Guid> to string</span></span>
* <span data-ttu-id="2124c-257">Преобразование <xref:System.Net.IPAddress> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-257">Converting <xref:System.Net.IPAddress> properties:</span></span>
  * <span data-ttu-id="2124c-258"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.IPAddressToBytesConverter> - <xref:System.Net.IPAddress> в массив байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-258"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.IPAddressToBytesConverter> - <xref:System.Net.IPAddress> to byte array</span></span>
  * <span data-ttu-id="2124c-259"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.IPAddressToStringConverter> - <xref:System.Net.IPAddress> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-259"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.IPAddressToStringConverter> - <xref:System.Net.IPAddress> to string</span></span>
* <span data-ttu-id="2124c-260">Преобразование числовых (int, Double, десятичного и т. д.) свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-260">Converting numeric (int, double, decimal, etc.) properties:</span></span>
  * <span data-ttu-id="2124c-261"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.NumberToBytesConverter%601> — Любое числовое значение массива байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-261"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.NumberToBytesConverter%601> - Any numerical value to byte array</span></span>
  * <span data-ttu-id="2124c-262"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.NumberToStringConverter%601> — Любое числовое значение в строке</span><span class="sxs-lookup"><span data-stu-id="2124c-262"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.NumberToStringConverter%601> - Any numerical value to string</span></span>
* <span data-ttu-id="2124c-263">Преобразование <xref:System.Net.NetworkInformation.PhysicalAddress> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-263">Converting <xref:System.Net.NetworkInformation.PhysicalAddress> properties:</span></span>
  * <span data-ttu-id="2124c-264"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.PhysicalAddressToBytesConverter> - <xref:System.Net.NetworkInformation.PhysicalAddress> в массив байтов</span><span class="sxs-lookup"><span data-stu-id="2124c-264"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.PhysicalAddressToBytesConverter> - <xref:System.Net.NetworkInformation.PhysicalAddress> to byte array</span></span>
  * <span data-ttu-id="2124c-265"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.PhysicalAddressToStringConverter> - <xref:System.Net.NetworkInformation.PhysicalAddress> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-265"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.PhysicalAddressToStringConverter> - <xref:System.Net.NetworkInformation.PhysicalAddress> to string</span></span>
* <span data-ttu-id="2124c-266">Преобразование строковых свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-266">Converting string properties:</span></span>
  * <span data-ttu-id="2124c-267"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToBoolConverter> — Строки, такие как "Y" и "N", в bool</span><span class="sxs-lookup"><span data-stu-id="2124c-267"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToBoolConverter> - Strings such as "Y" and "N" to bool</span></span>
  * <span data-ttu-id="2124c-268"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToBytesConverter> — Строка — UTF8-байт</span><span class="sxs-lookup"><span data-stu-id="2124c-268"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToBytesConverter> - String to UTF8 bytes</span></span>
  * <span data-ttu-id="2124c-269"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToCharConverter> -Строка в символ</span><span class="sxs-lookup"><span data-stu-id="2124c-269"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToCharConverter> - String to character</span></span>
  * <span data-ttu-id="2124c-270"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToDateTimeConverter> -String в <xref:System.DateTime></span><span class="sxs-lookup"><span data-stu-id="2124c-270"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToDateTimeConverter> - String to <xref:System.DateTime></span></span>
  * <span data-ttu-id="2124c-271"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToDateTimeOffsetConverter> -String в <xref:System.DateTimeOffset></span><span class="sxs-lookup"><span data-stu-id="2124c-271"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToDateTimeOffsetConverter> - String to <xref:System.DateTimeOffset></span></span>
  * <span data-ttu-id="2124c-272"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToEnumConverter%601> -Строка для перечисления</span><span class="sxs-lookup"><span data-stu-id="2124c-272"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToEnumConverter%601> - String to enum</span></span>
  * <span data-ttu-id="2124c-273"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToGuidConverter> -String в <xref:System.Guid></span><span class="sxs-lookup"><span data-stu-id="2124c-273"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToGuidConverter> - String to <xref:System.Guid></span></span>
  * <span data-ttu-id="2124c-274"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToNumberConverter%601> -Строка в числовой тип</span><span class="sxs-lookup"><span data-stu-id="2124c-274"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToNumberConverter%601> - String to numeric type</span></span>
  * <span data-ttu-id="2124c-275"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToTimeSpanConverter> -String в <xref:System.TimeSpan></span><span class="sxs-lookup"><span data-stu-id="2124c-275"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToTimeSpanConverter> - String to <xref:System.TimeSpan></span></span>
  * <span data-ttu-id="2124c-276"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToUriConverter> -String в <xref:System.Uri></span><span class="sxs-lookup"><span data-stu-id="2124c-276"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToUriConverter> - String to <xref:System.Uri></span></span>
* <span data-ttu-id="2124c-277">Преобразование <xref:System.TimeSpan> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-277">Converting <xref:System.TimeSpan> properties:</span></span>
  * <span data-ttu-id="2124c-278"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.TimeSpanToStringConverter> - <xref:System.TimeSpan> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-278"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.TimeSpanToStringConverter> - <xref:System.TimeSpan> to string</span></span>
  * <span data-ttu-id="2124c-279"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.TimeSpanToTicksConverter> - <xref:System.TimeSpan> в такты</span><span class="sxs-lookup"><span data-stu-id="2124c-279"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.TimeSpanToTicksConverter> - <xref:System.TimeSpan> to ticks</span></span>
* <span data-ttu-id="2124c-280">Преобразование <xref:System.Uri> свойств:</span><span class="sxs-lookup"><span data-stu-id="2124c-280">Converting <xref:System.Uri> properties:</span></span>
  * <span data-ttu-id="2124c-281"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.UriToStringConverter> - <xref:System.Uri> в строку</span><span class="sxs-lookup"><span data-stu-id="2124c-281"><xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.UriToStringConverter> - <xref:System.Uri> to string</span></span>

<span data-ttu-id="2124c-282">Обратите внимание, что все встроенные преобразователи не имеют состояния, поэтому один экземпляр может быть безопасно совместно использоваться несколькими свойствами.</span><span class="sxs-lookup"><span data-stu-id="2124c-282">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="column-facets-and-mapping-hints"></a><span data-ttu-id="2124c-283">Аспекты столбцов и указания по сопоставлению</span><span class="sxs-lookup"><span data-stu-id="2124c-283">Column facets and mapping hints</span></span>

<span data-ttu-id="2124c-284">Некоторые типы баз данных имеют аспекты, которые изменяют, как хранятся данные.</span><span class="sxs-lookup"><span data-stu-id="2124c-284">Some database types have facets that modify how the data is stored.</span></span> <span data-ttu-id="2124c-285">Сюда входит следующее.</span><span class="sxs-lookup"><span data-stu-id="2124c-285">These include:</span></span>

* <span data-ttu-id="2124c-286">Точность и масштаб для десятичных значений и столбцов даты и времени</span><span class="sxs-lookup"><span data-stu-id="2124c-286">Precision and scale for decimals and date/time columns</span></span>
* <span data-ttu-id="2124c-287">Размер и длина для двоичных и строковых столбцов</span><span class="sxs-lookup"><span data-stu-id="2124c-287">Size/length for binary and string columns</span></span>
* <span data-ttu-id="2124c-288">Юникод для строковых столбцов</span><span class="sxs-lookup"><span data-stu-id="2124c-288">Unicode for string columns</span></span>

<span data-ttu-id="2124c-289">Эти аспекты можно настроить обычным способом для свойства, которое использует преобразователь значений, и будет применяться к преобразованному типу базы данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-289">These facets can be configured in the normal way for a property that uses a value converter, and will apply to the converted database type.</span></span> <span data-ttu-id="2124c-290">Например, при преобразовании из перечисления в строки можно указать, что столбец базы данных должен быть не в Юникоде, и хранить до 20 символов:</span><span class="sxs-lookup"><span data-stu-id="2124c-290">For example, when converting from an enum to strings, we can specify that the database column should be non-Unicode and store up to 20 characters:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                modelBuilder
                    .Entity<Rider>()
                    .Property(e => e.Mount)
                    .HasConversion<string>()
                    .HasMaxLength(20)
                    .IsUnicode(false);
            }
-->
[!code-csharp[ConversionByClrTypeWithFacets](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByClrTypeWithFacets)]

<span data-ttu-id="2124c-291">Или при явном создании преобразователя:</span><span class="sxs-lookup"><span data-stu-id="2124c-291">Or, when creating the converter explicitly:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var converter = new ValueConverter<EquineBeast, string>(
                    v => v.ToString(),
                    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

                modelBuilder
                    .Entity<Rider>()
                    .Property(e => e.Mount)
                    .HasConversion(converter)
                    .HasMaxLength(20)
                    .IsUnicode(false);
            }
-->
[!code-csharp[ConversionByConverterInstanceWithFacets](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByConverterInstanceWithFacets)]

<span data-ttu-id="2124c-292">`varchar(20)`При использовании EF Core миграций в SQL Server возникает столбец:</span><span class="sxs-lookup"><span data-stu-id="2124c-292">This results in a `varchar(20)` column when using EF Core migrations against SQL Server:</span></span>

```sql
CREATE TABLE [Rider] (
    [Id] int NOT NULL IDENTITY,
    [Mount] varchar(20) NOT NULL,
    CONSTRAINT [PK_Rider] PRIMARY KEY ([Id]));
```

<span data-ttu-id="2124c-293">Однако если по умолчанию все `EquineBeast` столбцы должны быть `varchar(20)` , эти сведения могут быть переданы преобразователю значений как <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ConverterMappingHints> .</span><span class="sxs-lookup"><span data-stu-id="2124c-293">However, if by default all `EquineBeast` columns should be `varchar(20)`, then this information can be given to the value converter as a <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ConverterMappingHints>.</span></span> <span data-ttu-id="2124c-294">Пример:</span><span class="sxs-lookup"><span data-stu-id="2124c-294">For example:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var converter = new ValueConverter<EquineBeast, string>(
                    v => v.ToString(),
                    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v),
                    new ConverterMappingHints(size: 20, unicode: false));

                modelBuilder
                    .Entity<Rider>()
                    .Property(e => e.Mount)
                    .HasConversion(converter);
            }
-->
[!code-csharp[ConversionByConverterInstanceWithMappingHints](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByConverterInstanceWithMappingHints)]

<span data-ttu-id="2124c-295">Теперь в любой момент, когда используется этот преобразователь, столбец базы данных будет содержать не Юникод с максимальной длиной 20.</span><span class="sxs-lookup"><span data-stu-id="2124c-295">Now any time this converter is used, the database column will be non-unicode with a max length of 20.</span></span> <span data-ttu-id="2124c-296">Однако это только указания, так как они переопределяются любыми аспектами, явно заданными в сопоставленном свойстве.</span><span class="sxs-lookup"><span data-stu-id="2124c-296">However, these are only hints since they are be overridden by any facets explicitly set on the mapped property.</span></span>

## <a name="examples"></a><span data-ttu-id="2124c-297">Примеры</span><span class="sxs-lookup"><span data-stu-id="2124c-297">Examples</span></span>

### <a name="simple-value-objects"></a><span data-ttu-id="2124c-298">Простые объекты значений</span><span class="sxs-lookup"><span data-stu-id="2124c-298">Simple value objects</span></span>

<span data-ttu-id="2124c-299">В этом примере используется простой тип для заключения в оболочку примитивного типа.</span><span class="sxs-lookup"><span data-stu-id="2124c-299">This example uses a simple type to wrap a primitive type.</span></span> <span data-ttu-id="2124c-300">Это может быть полезно, если необходимо, чтобы тип в модели был более конкретным (и, следовательно, более строго типизированным), чем тип-примитив.</span><span class="sxs-lookup"><span data-stu-id="2124c-300">This can be useful when you want the type in your model to be more specific (and hence more type-safe) than a primitive type.</span></span> <span data-ttu-id="2124c-301">В этом примере это тип `Dollars` , который заключает в оболочку десятичный примитив:</span><span class="sxs-lookup"><span data-stu-id="2124c-301">In this example, that type is `Dollars`, which wraps the decimal primitive:</span></span>

<!--
        public readonly struct Dollars
        {
            public Dollars(decimal amount) 
                => Amount = amount;
            
            public decimal Amount { get; }

            public override string ToString() 
                => $"${Amount}";
        }
-->
[!code-csharp[SimpleValueObject](../../../samples/core/Modeling/ValueConversions/SimpleValueObject.cs?name=SimpleValueObject)]

<span data-ttu-id="2124c-302">Его можно использовать в типе сущности:</span><span class="sxs-lookup"><span data-stu-id="2124c-302">This can be used in an entity type:</span></span>

<!--
        public class Order
        {
            public int Id { get; set; }

            public Dollars Price { get; set; }
        }
-->
[!code-csharp[SimpleValueObjectModel](../../../samples/core/Modeling/ValueConversions/SimpleValueObject.cs?name=SimpleValueObjectModel)]

<span data-ttu-id="2124c-303">И преобразуются в базовый объект `decimal` при хранении в базе данных:</span><span class="sxs-lookup"><span data-stu-id="2124c-303">And converted to the underlying `decimal` when stored in the database:</span></span>

<!--
                modelBuilder.Entity<Order>()
                    .Property(e => e.Price)
                    .HasConversion(
                        v => v.Amount,
                        v => new Dollars(v));
-->
[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/SimpleValueObject.cs?name=ConfigureImmutableStructProperty)]

> [!NOTE]
> <span data-ttu-id="2124c-304">Этот объект значения реализуется как [Структура только для чтения](/dotnet/csharp/language-reference/builtin-types/struct).</span><span class="sxs-lookup"><span data-stu-id="2124c-304">This value object is implemented as a [readonly struct](/dotnet/csharp/language-reference/builtin-types/struct).</span></span> <span data-ttu-id="2124c-305">Это означает, что EF Core может выполнять моментальный снимок и сравнивать значения без проблем.</span><span class="sxs-lookup"><span data-stu-id="2124c-305">This means that EF Core can snapshot and compare values without issue.</span></span> <span data-ttu-id="2124c-306">Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .</span><span class="sxs-lookup"><span data-stu-id="2124c-306">See [Value Comparers](xref:core/modeling/value-comparers) for more information.</span></span>

### <a name="composite-value-objects"></a><span data-ttu-id="2124c-307">Составные объекты значений</span><span class="sxs-lookup"><span data-stu-id="2124c-307">Composite value objects</span></span>

<span data-ttu-id="2124c-308">В предыдущем примере тип объекта значения содержал только одно свойство.</span><span class="sxs-lookup"><span data-stu-id="2124c-308">In the previous example, the value object type contained only a single property.</span></span> <span data-ttu-id="2124c-309">Чаще всего тип объекта значения состоит из нескольких свойств, совместно образующих понятие предметной области.</span><span class="sxs-lookup"><span data-stu-id="2124c-309">It is more common for a value object type to compose multiple properties that together form a domain concept.</span></span> <span data-ttu-id="2124c-310">Например, общий `Money` тип, который содержит как сумму, так и валюту:</span><span class="sxs-lookup"><span data-stu-id="2124c-310">For example, a general `Money` type that contains both the amount and the currency:</span></span>

<!--
        public readonly struct Money
        {
            [JsonConstructor]
            public Money(decimal amount, Currency currency)
            {
                Amount = amount;
                Currency = currency;
            }

            public override string ToString()
                => (Currency == Currency.UsDollars ? "$" : "£") + Amount;

            public decimal Amount { get; }
            public Currency Currency { get; }
        }

        public enum Currency
        {
            UsDollars,
            PoundsStirling
        }
-->
[!code-csharp[CompositeValueObject](../../../samples/core/Modeling/ValueConversions/CompositeValueObject.cs?name=CompositeValueObject)]

<span data-ttu-id="2124c-311">Этот объект значения можно использовать в типе сущности, как и раньше:</span><span class="sxs-lookup"><span data-stu-id="2124c-311">This value object can be used in an entity type as before:</span></span>

<!--
        public class Order
        {
            public int Id { get; set; }

            public Money Price { get; set; }
        }
-->
[!code-csharp[CompositeValueObjectModel](../../../samples/core/Modeling/ValueConversions/CompositeValueObject.cs?name=CompositeValueObjectModel)]

<span data-ttu-id="2124c-312">В настоящее время преобразователи значений могут только преобразовывать значения в один столбец базы данных и из него.</span><span class="sxs-lookup"><span data-stu-id="2124c-312">Value converters can currently only convert values to and from a single database column.</span></span> <span data-ttu-id="2124c-313">Это ограничение означает, что все значения свойств объекта должны быть закодированы в одно значение столбца.</span><span class="sxs-lookup"><span data-stu-id="2124c-313">This limitation means that all property values from the object must be encoded into a single column value.</span></span> <span data-ttu-id="2124c-314">Обычно это обрабатывается путем сериализации объекта в базу данных и последующей десериализации. Например, с помощью <xref:System.Text.Json> :</span><span class="sxs-lookup"><span data-stu-id="2124c-314">This is typically handled by serializing the object as it goes into the database, and then deserializing it again on the way out. For example, using <xref:System.Text.Json>:</span></span>

<!--
                modelBuilder.Entity<Order>()
                    .Property(e => e.Price)
                    .HasConversion(
                        v => JsonSerializer.Serialize(v, null),
                        v => JsonSerializer.Deserialize<Money>(v, null));
-->
[!code-csharp[ConfigureCompositeValueObject](../../../samples/core/Modeling/ValueConversions/CompositeValueObject.cs?name=ConfigureCompositeValueObject)]

> [!NOTE]
> <span data-ttu-id="2124c-315">Мы планируем Разрешить сопоставление объекта с несколькими столбцами в EF Core 6,0, избавляя от необходимости использовать сериализацию здесь.</span><span class="sxs-lookup"><span data-stu-id="2124c-315">We plan to allow mapping an object to multiple columns in EF Core 6.0, removing the need to use serialization here.</span></span> <span data-ttu-id="2124c-316">Это осуществляется с помощью [#13947 проблем GitHub](https://github.com/dotnet/efcore/issues/13947).</span><span class="sxs-lookup"><span data-stu-id="2124c-316">This is tracked by [GitHub issue #13947](https://github.com/dotnet/efcore/issues/13947).</span></span>

> [!NOTE]
> <span data-ttu-id="2124c-317">Как и в предыдущем примере, этот объект значения реализуется как [Структура только для чтения](/dotnet/csharp/language-reference/builtin-types/struct).</span><span class="sxs-lookup"><span data-stu-id="2124c-317">As with the previous example, this value object is implemented as a [readonly struct](/dotnet/csharp/language-reference/builtin-types/struct).</span></span> <span data-ttu-id="2124c-318">Это означает, что EF Core может выполнять моментальный снимок и сравнивать значения без проблем.</span><span class="sxs-lookup"><span data-stu-id="2124c-318">This means that EF Core can snapshot and compare values without issue.</span></span> <span data-ttu-id="2124c-319">Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .</span><span class="sxs-lookup"><span data-stu-id="2124c-319">See [Value Comparers](xref:core/modeling/value-comparers) for more information.</span></span>

### <a name="collections-of-primitives"></a><span data-ttu-id="2124c-320">Коллекции примитивов</span><span class="sxs-lookup"><span data-stu-id="2124c-320">Collections of primitives</span></span>

<span data-ttu-id="2124c-321">Сериализация также может использоваться для хранения коллекции примитивных значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-321">Serialization can also be used to store a collection of primitive values.</span></span> <span data-ttu-id="2124c-322">Пример:</span><span class="sxs-lookup"><span data-stu-id="2124c-322">For example:</span></span>

<!--
        public class Post
        {
            public int Id { get; set; }
            public string Title { get; set; }
            public string Contents { get; set; }

            public ICollection<string> Tags { get; set; }
        }
-->
[!code-csharp[PrimitiveCollectionModel](../../../samples/core/Modeling/ValueConversions/PrimitiveCollection.cs?name=PrimitiveCollectionModel)]

<span data-ttu-id="2124c-323"><xref:System.Text.Json>Повторное использование:</span><span class="sxs-lookup"><span data-stu-id="2124c-323">Using <xref:System.Text.Json> again:</span></span>

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.Tags)
                    .HasConversion(
                        v => JsonSerializer.Serialize(v, null),
                        v => JsonSerializer.Deserialize<List<string>>(v, null),
                        new ValueComparer<ICollection<string>>(
                            (c1, c2) => c1.SequenceEqual(c2),
                            c => c.Aggregate(0, (a, v) => HashCode.Combine(a, v.GetHashCode())),
                            c => (ICollection<string>)c.ToList()));
-->
[!code-csharp[ConfigurePrimitiveCollection](../../../samples/core/Modeling/ValueConversions/PrimitiveCollection.cs?name=ConfigurePrimitiveCollection)]

<span data-ttu-id="2124c-324">`ICollection<string>` Представляет изменяемый ссылочный тип.</span><span class="sxs-lookup"><span data-stu-id="2124c-324">`ICollection<string>` represents a mutable reference type.</span></span> <span data-ttu-id="2124c-325">Это означает, что <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> необходимо, чтобы EF Core могли правильно отслеживать и обнаруживать изменения.</span><span class="sxs-lookup"><span data-stu-id="2124c-325">This means that a <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> is needed so that EF Core can track and detect changes correctly.</span></span> <span data-ttu-id="2124c-326">Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .</span><span class="sxs-lookup"><span data-stu-id="2124c-326">See [Value Comparers](xref:core/modeling/value-comparers) for more information.</span></span>

### <a name="collections-of-value-objects"></a><span data-ttu-id="2124c-327">Коллекции объектов значений</span><span class="sxs-lookup"><span data-stu-id="2124c-327">Collections of value objects</span></span>

<span data-ttu-id="2124c-328">Объединяя два предыдущих примера, мы можем создать коллекцию объектов значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-328">Combining the previous two examples together we can create a collection of value objects.</span></span> <span data-ttu-id="2124c-329">Например, рассмотрим `AnnualFinance` тип, который моделирует финансы в блоге в течение одного года:</span><span class="sxs-lookup"><span data-stu-id="2124c-329">For example, consider an `AnnualFinance` type that models blog finances for a single year:</span></span>

<!--
        public readonly struct AnnualFinance
        {
            [JsonConstructor]
            public AnnualFinance(int year, Money income, Money expenses)
            {
                Year = year;
                Income = income;
                Expenses = expenses;
            }

            public int Year { get; }
            public Money Income { get; }
            public Money Expenses { get; }
            public Money Revenue => new Money(Income.Amount - Expenses.Amount, Income.Currency);
        }
-->
[!code-csharp[ValueObjectCollection](../../../samples/core/Modeling/ValueConversions/ValueObjectCollection.cs?name=ValueObjectCollection)]

<span data-ttu-id="2124c-330">Этот тип состоит из нескольких `Money` созданных ранее типов:</span><span class="sxs-lookup"><span data-stu-id="2124c-330">This type composes several of the `Money` types we created previously:</span></span>

<!--
        public readonly struct Money
        {
            [JsonConstructor]
            public Money(decimal amount, Currency currency)
            {
                Amount = amount;
                Currency = currency;
            }

            public override string ToString()
                => (Currency == Currency.UsDollars ? "$" : "£") + Amount;

            public decimal Amount { get; }
            public Currency Currency { get; }
        }

        public enum Currency
        {
            UsDollars,
            PoundsStirling
        }
-->
[!code-csharp[ValueObjectCollectionMoney](../../../samples/core/Modeling/ValueConversions/ValueObjectCollection.cs?name=ValueObjectCollectionMoney)]

<span data-ttu-id="2124c-331">Затем можно добавить коллекцию `AnnualFinance` в наш тип сущности:</span><span class="sxs-lookup"><span data-stu-id="2124c-331">We can then add a collection of `AnnualFinance` to our entity type:</span></span>

<!--
        public class Blog
        {
            public int Id { get; set; }
            public string Name { get; set; }
            
            public IList<AnnualFinance> Finances { get; set; }
        }
-->
[!code-csharp[ValueObjectCollectionModel](../../../samples/core/Modeling/ValueConversions/ValueObjectCollection.cs?name=ValueObjectCollectionModel)]

<span data-ttu-id="2124c-332">И снова Используйте сериализацию для хранения этого:</span><span class="sxs-lookup"><span data-stu-id="2124c-332">And again use serialization to store this:</span></span>

<!--
                modelBuilder.Entity<Blog>()
                    .Property(e => e.Finances)
                    .HasConversion(
                        v => JsonSerializer.Serialize(v, null),
                        v => JsonSerializer.Deserialize<List<AnnualFinance>>(v, null),
                        new ValueComparer<IList<AnnualFinance>>(
                            (c1, c2) => c1.SequenceEqual(c2),
                            c => c.Aggregate(0, (a, v) => HashCode.Combine(a, v.GetHashCode())),
                            c => (IList<AnnualFinance>)c.ToList()));
-->
[!code-csharp[ConfigureValueObjectCollection](../../../samples/core/Modeling/ValueConversions/ValueObjectCollection.cs?name=ConfigureValueObjectCollection)]

> [!NOTE]
> <span data-ttu-id="2124c-333">Как и раньше, для этого преобразования требуется <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> .</span><span class="sxs-lookup"><span data-stu-id="2124c-333">As before, this conversion requires a <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601>.</span></span> <span data-ttu-id="2124c-334">Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .</span><span class="sxs-lookup"><span data-stu-id="2124c-334">See [Value Comparers](xref:core/modeling/value-comparers) for more information.</span></span>

### <a name="value-objects-as-keys"></a><span data-ttu-id="2124c-335">Объекты значений в качестве ключей</span><span class="sxs-lookup"><span data-stu-id="2124c-335">Value objects as keys</span></span>

<span data-ttu-id="2124c-336">Иногда примитивные свойства ключа могут быть заключены в объекты значений для добавления дополнительного уровня безопасности типа при присвоении значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-336">Sometimes primitive key properties may be wrapped in value objects to add an additional level of type-safety in assigning values.</span></span> <span data-ttu-id="2124c-337">Например, можно реализовать тип ключа для блогов и тип ключа для записей:</span><span class="sxs-lookup"><span data-stu-id="2124c-337">For example, we could implement a key type for blogs, and a key type for posts:</span></span>

<!--
        public readonly struct BlogKey
        {
            public BlogKey(int id) => Id = id;
            public int Id { get; }
        }

        public readonly struct PostKey
        {
            public PostKey(int id) => Id = id;
            public int Id { get; }
        }
-->
[!code-csharp[KeyValueObjects](../../../samples/core/Modeling/ValueConversions/KeyValueObjects.cs?name=KeyValueObjects)]

<span data-ttu-id="2124c-338">Затем их можно использовать в модели предметной области:</span><span class="sxs-lookup"><span data-stu-id="2124c-338">These can then be used in the domain model:</span></span>

<!--
        public class Blog
        {
            public BlogKey Id { get; set; }
            public string Name { get; set; }

            public ICollection<Post> Posts { get; set; }
        }

        public class Post
        {
            public PostKey Id { get; set; }

            public string Title { get; set; }
            public string Content { get; set; }

            public BlogKey? BlogId { get; set; }
            public Blog Blog { get; set; }
        }
-->
[!code-csharp[KeyValueObjectsModel](../../../samples/core/Modeling/ValueConversions/KeyValueObjects.cs?name=KeyValueObjectsModel)]

<span data-ttu-id="2124c-339">Обратите внимание, что `Blog.Id` невозможно случайно присвоить значение `PostKey` и `Post.Id` случайно назначить `BlogKey` .</span><span class="sxs-lookup"><span data-stu-id="2124c-339">Notice that `Blog.Id` cannot accidentally be assigned a `PostKey`, and `Post.Id` cannot accidentally be assigned a `BlogKey`.</span></span> <span data-ttu-id="2124c-340">Аналогичным образом `Post.BlogId` свойству внешнего ключа должно быть присвоено значение `BlogKey` .</span><span class="sxs-lookup"><span data-stu-id="2124c-340">Similarly, the `Post.BlogId` foreign key property must be assigned a `BlogKey`.</span></span>

> [!NOTE]
> <span data-ttu-id="2124c-341">Отображение этого шаблона не означает, что мы рекомендуем его.</span><span class="sxs-lookup"><span data-stu-id="2124c-341">Showing this pattern does not mean we recommend it.</span></span> <span data-ttu-id="2124c-342">Тщательно определите, помогает ли этот уровень абстракции помочь или препятствовать разработке.</span><span class="sxs-lookup"><span data-stu-id="2124c-342">Carefully consider whether this level of abstraction is helping or hampering your development experience.</span></span> <span data-ttu-id="2124c-343">Кроме того, рассмотрите возможность использования переходов и созданных ключей вместо непосредственного обращения к ключевым значениям.</span><span class="sxs-lookup"><span data-stu-id="2124c-343">Also, consider using navigations and generated keys instead of dealing with key values directly.</span></span>

<span data-ttu-id="2124c-344">Эти ключевые свойства затем можно сопоставить с помощью преобразователей значений:</span><span class="sxs-lookup"><span data-stu-id="2124c-344">These key properties can then be mapped using value converters:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var blogKeyConverter = new ValueConverter<BlogKey, int>(
                    v => v.Id,
                    v => new BlogKey(v));

                modelBuilder.Entity<Blog>().Property(e => e.Id).HasConversion(blogKeyConverter);

                modelBuilder.Entity<Post>(
                    b =>
                        {
                            b.Property(e => e.Id).HasConversion(v => v.Id, v => new PostKey(v));
                            b.Property(e => e.BlogId).HasConversion(blogKeyConverter);
                        });
            }
-->
[!code-csharp[ConfigureKeyValueObjects](../../../samples/core/Modeling/ValueConversions/KeyValueObjects.cs?name=ConfigureKeyValueObjects)]

> [!NOTE]
> <span data-ttu-id="2124c-345">В настоящее время ключевые свойства с преобразованиями не могут использовать созданные значения ключа.</span><span class="sxs-lookup"><span data-stu-id="2124c-345">Currently key properties with conversions cannot use generated key values.</span></span> <span data-ttu-id="2124c-346">Проголосуйте за [#11597 проблем GitHub](https://github.com/dotnet/efcore/issues/11597) , чтобы это ограничение было удалено.</span><span class="sxs-lookup"><span data-stu-id="2124c-346">Vote for [GitHub issue #11597](https://github.com/dotnet/efcore/issues/11597) to have this limitation removed.</span></span>

### <a name="use-ulong-for-timestamprowversion"></a><span data-ttu-id="2124c-347">Использование ULong для timestamp/rowversion</span><span class="sxs-lookup"><span data-stu-id="2124c-347">Use ulong for timestamp/rowversion</span></span>

<span data-ttu-id="2124c-348">SQL Server поддерживает автоматический [оптимистичный параллелизм](xref:core/saving/concurrency) с использованием [8-байтных двоичных `rowversion` / `timestamp` столбцов](/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="2124c-348">SQL Server supports automatic [optimistic concurrency](xref:core/saving/concurrency) using [8-byte binary `rowversion`/`timestamp` columns](/sql/t-sql/data-types/rowversion-transact-sql).</span></span> <span data-ttu-id="2124c-349">Они всегда считываются из базы данных и записываются в нее с помощью 8-байтового массива.</span><span class="sxs-lookup"><span data-stu-id="2124c-349">These are always read from and written to the database using an 8-byte array.</span></span> <span data-ttu-id="2124c-350">Однако массивы байтов являются изменяемыми ссылочными типами, что делает их довольно ненужными для работы.</span><span class="sxs-lookup"><span data-stu-id="2124c-350">However, byte arrays are a mutable reference type, which makes them somewhat painful to deal with.</span></span> <span data-ttu-id="2124c-351">Преобразователи значений позволяют `rowversion` вместо этого сопоставляться со `ulong` свойством, что является гораздо более подходящим и простым в использовании, чем массив байтов.</span><span class="sxs-lookup"><span data-stu-id="2124c-351">Value converters allow the `rowversion` to instead be mapped to a `ulong` property, which is much more appropriate and easy to use than the byte array.</span></span> <span data-ttu-id="2124c-352">Например, рассмотрим `Blog` сущность с маркером параллелизма ulong:</span><span class="sxs-lookup"><span data-stu-id="2124c-352">For example, consider a `Blog` entity with a ulong concurrency token:</span></span>

<!--
        public class Blog
        {
            public int Id { get; set; }
            public string Name { get; set; }
            public ulong Version { get; set; }
        }
-->
[!code-csharp[ULongConcurrencyModel](../../../samples/core/Modeling/ValueConversions/ULongConcurrency.cs?name=ULongConcurrencyModel)]

<span data-ttu-id="2124c-353">Это можно сопоставить со столбцом SQL Server `rowversion` с помощью преобразователя значений:</span><span class="sxs-lookup"><span data-stu-id="2124c-353">This can be mapped to a SQL server `rowversion` column using a value converter:</span></span>

<!--
                modelBuilder.Entity<Blog>()
                    .Property(e => e.Version)
                    .IsRowVersion()
                    .HasConversion<byte[]>();
-->
[!code-csharp[ConfigureULongConcurrency](../../../samples/core/Modeling/ValueConversions/ULongConcurrency.cs?name=ConfigureULongConcurrency)]

### <a name="specify-the-datetimekind-when-reading-dates"></a><span data-ttu-id="2124c-354">Укажите DateTime. Kind при чтении дат</span><span class="sxs-lookup"><span data-stu-id="2124c-354">Specify the DateTime.Kind when reading dates</span></span>

<span data-ttu-id="2124c-355">SQL Server отменяет <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> флаг при сохранении в <xref:System.DateTime> виде [`datetime`](/sql/t-sql/data-types/datetime-transact-sql) или [`datetime2`](/sql/t-sql/data-types/datetime2-transact-sql) .</span><span class="sxs-lookup"><span data-stu-id="2124c-355">SQL Server discards the <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> flag when storing a <xref:System.DateTime> as a [`datetime`](/sql/t-sql/data-types/datetime-transact-sql) or [`datetime2`](/sql/t-sql/data-types/datetime2-transact-sql).</span></span> <span data-ttu-id="2124c-356">Это означает, что значения DateTime, поступившие из базы данных, всегда имеют значение <xref:System.DateTimeKind> `Unspecified` .</span><span class="sxs-lookup"><span data-stu-id="2124c-356">This means that DateTime values coming back from the database always have a <xref:System.DateTimeKind> of `Unspecified`.</span></span>

<span data-ttu-id="2124c-357">Преобразователи значений можно использовать двумя способами для решения этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="2124c-357">Value converters can be used in two ways to deal with this.</span></span> <span data-ttu-id="2124c-358">Во-первых, EF Core имеет преобразователь значений, который создает 8-байтовое непрозрачное значение, которое сохраняет `Kind` флаг.</span><span class="sxs-lookup"><span data-stu-id="2124c-358">First, EF Core has a value converter that creates an 8-byte opaque value which preserves the `Kind` flag.</span></span> <span data-ttu-id="2124c-359">Пример:</span><span class="sxs-lookup"><span data-stu-id="2124c-359">For example:</span></span>

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.PostedOn)
                    .HasConversion<long>();
-->
[!code-csharp[ConfigurePreserveDateTimeKind1](../../../samples/core/Modeling/ValueConversions/PreserveDateTimeKind.cs?name=ConfigurePreserveDateTimeKind1)]

<span data-ttu-id="2124c-360">Это позволяет смешивать значения DateTime с различными `Kind` флагами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-360">This allows DateTime values with different `Kind` flags to be mixed in the database.</span></span>

<span data-ttu-id="2124c-361">Проблема с этим подходом заключается в том, что база данных больше не может распознать или не иметь распознаваемых `datetime` `datetime2` столбцов.</span><span class="sxs-lookup"><span data-stu-id="2124c-361">The problem with this approach is that the database no longer has recognizable `datetime` or `datetime2` columns.</span></span> <span data-ttu-id="2124c-362">Поэтому обычно всегда следует хранить время в формате UTC (или, реже, всегда местное время), а затем либо игнорировать флаг, `Kind` либо присвоить ему соответствующее значение с помощью преобразователя значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-362">So instead it is common to always store UTC time (or, less commonly, always local time) and then either ignore the `Kind` flag or set it to the appropriate value using a value converter.</span></span> <span data-ttu-id="2124c-363">Например, приведенный ниже преобразователь гарантирует, что `DateTime` значение, считанное из базы данных, будет иметь <xref:System.DateTimeKind> `UTC` :</span><span class="sxs-lookup"><span data-stu-id="2124c-363">For example, the converter below ensures that the `DateTime` value read from the database will have the <xref:System.DateTimeKind> `UTC`:</span></span>

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.LastUpdated)
                    .HasConversion(
                        v => v,
                        v => new DateTime(v.Ticks, DateTimeKind.Utc));
-->
[!code-csharp[ConfigurePreserveDateTimeKind2](../../../samples/core/Modeling/ValueConversions/PreserveDateTimeKind.cs?name=ConfigurePreserveDateTimeKind2)]

<span data-ttu-id="2124c-364">Если в экземплярах сущностей задаются смешанные значения в формате UTC, то перед вставкой можно использовать преобразователь для правильного преобразования.</span><span class="sxs-lookup"><span data-stu-id="2124c-364">If a mix of local and UTC values are being set in entity instances, then the converter can be used to convert appropriately before inserting.</span></span> <span data-ttu-id="2124c-365">Пример:</span><span class="sxs-lookup"><span data-stu-id="2124c-365">For example:</span></span>

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.LastUpdated)
                    .HasConversion(
                        v => v.ToUniversalTime(),
                        v => new DateTime(v.Ticks, DateTimeKind.Utc));
-->
[!code-csharp[ConfigurePreserveDateTimeKind3](../../../samples/core/Modeling/ValueConversions/PreserveDateTimeKind.cs?name=ConfigurePreserveDateTimeKind3)]

> [!NOTE]
> <span data-ttu-id="2124c-366">Тщательно рассмотрите возможность объединить весь код доступа к базе данных, чтобы использовать время в формате UTC, только используя местное время при представлении данных для пользователей.</span><span class="sxs-lookup"><span data-stu-id="2124c-366">Carefully consider unifying all database access code to use UTC time all the time, only dealing with local time when presenting data to users.</span></span>

### <a name="use-case-insensitive-string-keys"></a><span data-ttu-id="2124c-367">Использовать строковые ключи без учета регистра</span><span class="sxs-lookup"><span data-stu-id="2124c-367">Use case-insensitive string keys</span></span>

<span data-ttu-id="2124c-368">Некоторые базы данных, в том числе SQL Server, выполняют сравнения строк без учета регистра по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2124c-368">Some databases, including SQL Server, perform case-insensitive string comparisons by default.</span></span> <span data-ttu-id="2124c-369">.NET, с другой стороны, выполняет сравнение строк с учетом регистра по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2124c-369">.NET, on the other hand, performs case-sensitive string comparisons by default.</span></span> <span data-ttu-id="2124c-370">Это означает, что значение внешнего ключа, например DotNet, будет соответствовать значению "DotNet" первичного ключа на SQL Server, но не будет соответствовать этому значению в EF Core.</span><span class="sxs-lookup"><span data-stu-id="2124c-370">This means that a foreign key value like "DotNet" will match the primary key value "dotnet" on SQL Server, but will not match it in EF Core.</span></span> <span data-ttu-id="2124c-371">Функцию сравнения значений для ключей можно использовать для принудительного EF Core в сравнении строк без учета регистра, например в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-371">A value comparer for keys can be used to force EF Core into case-insensitive string comparisons like in the database.</span></span> <span data-ttu-id="2124c-372">Например, рассмотрим модель записи блога/posts со строковыми ключами:</span><span class="sxs-lookup"><span data-stu-id="2124c-372">For example, consider a blog/posts model with string keys:</span></span>

<!--
        public class Blog
        {
            public string Id { get; set; }
            public string Name { get; set; }

            public ICollection<Post> Posts { get; set; }
        }

        public class Post
        {
            public string Id { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public string BlogId { get; set; }
            public Blog Blog { get; set; }
        }
-->
[!code-csharp[CaseInsensitiveStringsModel](../../../samples/core/Modeling/ValueConversions/CaseInsensitiveStrings.cs?name=CaseInsensitiveStringsModel)]

<span data-ttu-id="2124c-373">Это не будет работать должным образом, если некоторые из `Post.BlogId` значений имеют разный регистр.</span><span class="sxs-lookup"><span data-stu-id="2124c-373">This will not work as expected if some of the `Post.BlogId` values have different casing.</span></span> <span data-ttu-id="2124c-374">Ошибки, вызванные этим действием, зависят от того, что делает приложение, но обычно содержат графы объектов, которые не были правильно [исправлены](xref:core/change-tracking/relationship-changes) , и (или) обновления, которые завершаются неудачей из-за неправильного значения FK.</span><span class="sxs-lookup"><span data-stu-id="2124c-374">The errors caused by this will depend on what the application is doing, but typically involve graphs of objects that are not [fixed-up](xref:core/change-tracking/relationship-changes) correctly, and/or updates that fail because the FK value is wrong.</span></span> <span data-ttu-id="2124c-375">Чтобы исправить это, можно использовать компаратор значений:</span><span class="sxs-lookup"><span data-stu-id="2124c-375">A value comparer can be used to correct this:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var comparer = new ValueComparer<string>(
                    (l, r) => string.Equals(l, r, StringComparison.OrdinalIgnoreCase),
                    v => v.ToUpper().GetHashCode(),
                    v => v);

                modelBuilder.Entity<Blog>()
                    .Property(e => e.Id)
                    .Metadata.SetValueComparer(comparer);

                modelBuilder.Entity<Post>(
                    b =>
                        {
                            b.Property(e => e.Id).Metadata.SetValueComparer(comparer);
                            b.Property(e => e.BlogId).Metadata.SetValueComparer(comparer);
                        });
            }
-->
[!code-csharp[ConfigureCaseInsensitiveStrings](../../../samples/core/Modeling/ValueConversions/CaseInsensitiveStrings.cs?name=ConfigureCaseInsensitiveStrings)]

> [!NOTE]
> <span data-ttu-id="2124c-376">Сравнения строк .NET и сравнения строк базы данных могут различаться не только с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="2124c-376">.NET string comparisons and database string comparisons can differ in more than just case sensitivity.</span></span> <span data-ttu-id="2124c-377">Этот шаблон работает для простых ключей ASCII, но может завершиться ошибкой для ключей с любым видом символов, относящихся к региональным параметрам.</span><span class="sxs-lookup"><span data-stu-id="2124c-377">This pattern works for simple ASCII keys, but may fail for keys with any kind of culture-specific characters.</span></span> <span data-ttu-id="2124c-378">Дополнительные сведения см. в разделе [Параметры сортировки и чувствительность к регистру](xref:core/miscellaneous/collations-and-case-sensitivity) .</span><span class="sxs-lookup"><span data-stu-id="2124c-378">See [Collations and Case Sensitivity](xref:core/miscellaneous/collations-and-case-sensitivity) for more information.</span></span>

### <a name="handle-fixed-length-database-strings"></a><span data-ttu-id="2124c-379">Обработайте строки базы данных фиксированной длины</span><span class="sxs-lookup"><span data-stu-id="2124c-379">Handle fixed-length database strings</span></span>

<span data-ttu-id="2124c-380">В предыдущем примере не требуется преобразователь значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-380">The previous example did not need a value converter.</span></span> <span data-ttu-id="2124c-381">Однако преобразователь может быть полезен для строковых типов базы данных фиксированной длины `char(20)` , таких как или `nchar(20)` .</span><span class="sxs-lookup"><span data-stu-id="2124c-381">However, a converter can be useful for fixed-length database string types like `char(20)` or `nchar(20)`.</span></span> <span data-ttu-id="2124c-382">Строки фиксированной длины дополняются до полной длины при вставке значения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-382">Fixed-length strings are padded to their full length whenever a value is inserted into the database.</span></span> <span data-ttu-id="2124c-383">Это означает, что значение ключа " `dotnet` " будет считаться обратно из базы данных как " `dotnet..............` ", где `.` представляет символ пробела.</span><span class="sxs-lookup"><span data-stu-id="2124c-383">This means that a key value of "`dotnet`" will be read back from the database as "`dotnet..............`", where `.` represents a space character.</span></span> <span data-ttu-id="2124c-384">После этого они не будут правильно сравниваться с ключевыми значениями, которые не являются дополненными.</span><span class="sxs-lookup"><span data-stu-id="2124c-384">This will then not compare correctly with key values that are not padded.</span></span>

<span data-ttu-id="2124c-385">Преобразователь значений можно использовать для усечения заполнения при считывании значений ключа.</span><span class="sxs-lookup"><span data-stu-id="2124c-385">A value converter can be used to trim the padding when reading key values.</span></span> <span data-ttu-id="2124c-386">Это можно сочетать с функцией сравнения значений в предыдущем примере для правильного сравнения ключей ASCII с фиксированной длиной без учета регистра.</span><span class="sxs-lookup"><span data-stu-id="2124c-386">This can be combined with the value comparer in the previous example to compare fixed length case-insensitive ASCII keys correctly.</span></span> <span data-ttu-id="2124c-387">Пример:</span><span class="sxs-lookup"><span data-stu-id="2124c-387">For example:</span></span>

<!--
            protected override void OnModelCreating(ModelBuilder modelBuilder)
            {
                var converter = new ValueConverter<string, string>(
                    v => v,
                    v => v.Trim());
                
                var comparer = new ValueComparer<string>(
                    (l, r) => string.Equals(l, r, StringComparison.OrdinalIgnoreCase),
                    v => v.ToUpper().GetHashCode(),
                    v => v);

                modelBuilder.Entity<Blog>()
                    .Property(e => e.Id)
                    .HasColumnType("char(20)")
                    .HasConversion(converter, comparer);

                modelBuilder.Entity<Post>(
                    b =>
                        {
                            b.Property(e => e.Id).HasColumnType("char(20)").HasConversion(converter, comparer);
                            b.Property(e => e.BlogId).HasColumnType("char(20)").HasConversion(converter, comparer);
                        });
            }
-->
[!code-csharp[ConfigureFixedLengthStrings](../../../samples/core/Modeling/ValueConversions/FixedLengthStrings.cs?name=ConfigureFixedLengthStrings)]

### <a name="encrypt-property-values"></a><span data-ttu-id="2124c-388">Шифровать значения свойств</span><span class="sxs-lookup"><span data-stu-id="2124c-388">Encrypt property values</span></span>

<span data-ttu-id="2124c-389">Преобразователи значения можно использовать для шифрования значений свойств перед их отправкой в базу данных и последующей расшифровки. Например, при использовании в качестве замены для реального алгоритма шифрования строкового типа используется реверсирование.</span><span class="sxs-lookup"><span data-stu-id="2124c-389">Value converters can be used to encrypt property values before sending them to the database, and then decrypt them on the way out. For example, using string reversal as a substitute for a real encryption algorithm:</span></span>

<!--
                modelBuilder.Entity<User>().Property(e => e.Password).HasConversion(
                    v => new string(v.Reverse().ToArray()),
                    v => new string(v.Reverse().ToArray()));
-->
[!code-csharp[ConfigureEncryptPropertyValues](../../../samples/core/Modeling/ValueConversions/EncryptPropertyValues.cs?name=ConfigureEncryptPropertyValues)]

> [!NOTE]
> <span data-ttu-id="2124c-390">В настоящее время невозможно получить ссылку на текущий DbContext или другое состояние сеанса из преобразователя значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-390">There is currently no way to get a reference to the current DbContext, or other session state, from within a value converter.</span></span> <span data-ttu-id="2124c-391">Это ограничивает виды шифрования, которые можно использовать.</span><span class="sxs-lookup"><span data-stu-id="2124c-391">This limits the kinds of encryption that can be used.</span></span> <span data-ttu-id="2124c-392">Проголосуйте за [#11597 проблем GitHub](https://github.com/dotnet/efcore/issues/12205) , чтобы это ограничение было удалено.</span><span class="sxs-lookup"><span data-stu-id="2124c-392">Vote for [GitHub issue #11597](https://github.com/dotnet/efcore/issues/12205) to have this limitation removed.</span></span>

> [!WARNING]
> <span data-ttu-id="2124c-393">Не забудьте понять все последствия, если вы перепланируете собственное шифрование для защиты конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="2124c-393">Make sure to understand all the implications if you roll your own encryption to protect sensitive data.</span></span> <span data-ttu-id="2124c-394">Вместо этого рекомендуется использовать предварительно созданные механизмы шифрования, например [Always encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) на SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2124c-394">Consider instead using pre-built encryption mechanisms, such as [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) on SQL Server.</span></span>

## <a name="limitations"></a><span data-ttu-id="2124c-395">Ограничения</span><span class="sxs-lookup"><span data-stu-id="2124c-395">Limitations</span></span>

<span data-ttu-id="2124c-396">Существует несколько известных текущих ограничений системы преобразования значений:</span><span class="sxs-lookup"><span data-stu-id="2124c-396">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="2124c-397">В настоящее время нет способа указать в одном месте, что каждое свойство данного типа должно использовать один и тот же преобразователь значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-397">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="2124c-398">Ответьте ( 👍 ) на [вопрос GitHub #10784](https://github.com/dotnet/efcore/issues/10784) , если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="2124c-398">Please vote (👍) for [GitHub issue #10784](https://github.com/dotnet/efcore/issues/10784) if this is something you need.</span></span>
* <span data-ttu-id="2124c-399">Как отмечалось выше, `null` невозможно преобразовать.</span><span class="sxs-lookup"><span data-stu-id="2124c-399">As noted above, `null` cannot be converted.</span></span> <span data-ttu-id="2124c-400">Ответьте ( 👍 ) на [вопрос GitHub #13850](https://github.com/dotnet/efcore/issues/13850) , если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="2124c-400">Please vote (👍) for [GitHub issue #13850](https://github.com/dotnet/efcore/issues/13850) if this is something you need.</span></span>
* <span data-ttu-id="2124c-401">В настоящее время невозможно распределить преобразование одного свойства в несколько столбцов или наоборот.</span><span class="sxs-lookup"><span data-stu-id="2124c-401">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span> <span data-ttu-id="2124c-402">Ответьте ( 👍 ) на [вопрос GitHub #13947](https://github.com/dotnet/efcore/issues/13947) , если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="2124c-402">Please vote (👍) for [GitHub issue #13947](https://github.com/dotnet/efcore/issues/13947) if this is something you need.</span></span>
* <span data-ttu-id="2124c-403">Создание значения не поддерживается для большинства ключей, сопоставленных с помощью преобразователей значений.</span><span class="sxs-lookup"><span data-stu-id="2124c-403">Value generation is not supported for most keys mapped through value converters.</span></span> <span data-ttu-id="2124c-404">Ответьте ( 👍 ) на [вопрос GitHub #11597](https://github.com/dotnet/efcore/issues/11597) , если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="2124c-404">Please vote (👍) for [GitHub issue #11597](https://github.com/dotnet/efcore/issues/11597) if this is something you need.</span></span>
* <span data-ttu-id="2124c-405">Преобразования значений не могут ссылаться на текущий экземпляр DbContext.</span><span class="sxs-lookup"><span data-stu-id="2124c-405">Value conversions cannot reference the current DbContext instance.</span></span> <span data-ttu-id="2124c-406">Ответьте ( 👍 ) на [вопрос GitHub #11597](https://github.com/dotnet/efcore/issues/12205) , если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="2124c-406">Please vote (👍) for [GitHub issue #11597](https://github.com/dotnet/efcore/issues/12205) if this is something you need.</span></span>

<span data-ttu-id="2124c-407">Удаление этих ограничений рассматривается в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="2124c-407">Removal of these limitations is being considered for future releases.</span></span>
