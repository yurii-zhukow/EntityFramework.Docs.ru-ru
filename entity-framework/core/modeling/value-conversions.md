---
title: Преобразования значений — EF Core
description: Настройка преобразователей значений в модели Entity Framework Core
author: ajcvickers
ms.date: 02/19/2018
uid: core/modeling/value-conversions
ms.openlocfilehash: 221560a145fe25c2b7bf094839dd37791bc25955
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063963"
---
# <a name="value-conversions"></a><span data-ttu-id="16924-103">Преобразования значений</span><span class="sxs-lookup"><span data-stu-id="16924-103">Value Conversions</span></span>

<span data-ttu-id="16924-104">Преобразователи значений позволяют преобразовывать значения свойств при чтении из базы данных или записи в нее.</span><span class="sxs-lookup"><span data-stu-id="16924-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="16924-105">Это преобразование может быть из одного значения другого типа (например, для шифрования строк) или из значения одного типа в значение другого типа (например, преобразование перечисляемых значений в строки базы данных и из них).</span><span class="sxs-lookup"><span data-stu-id="16924-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="16924-106">Основы</span><span class="sxs-lookup"><span data-stu-id="16924-106">Fundamentals</span></span>

<span data-ttu-id="16924-107">Преобразователи значений указываются в терминах `ModelClrType` и `ProviderClrType` .</span><span class="sxs-lookup"><span data-stu-id="16924-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="16924-108">Тип модели — это тип .NET свойства в типе сущности.</span><span class="sxs-lookup"><span data-stu-id="16924-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="16924-109">Тип поставщика — это тип .NET, понятный поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="16924-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="16924-110">Например, чтобы сохранить перечисления в виде строк в базе данных, тип модели является типом перечисления, а тип поставщика — `String` .</span><span class="sxs-lookup"><span data-stu-id="16924-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="16924-111">Эти два типа могут быть одинаковыми.</span><span class="sxs-lookup"><span data-stu-id="16924-111">These two types can be the same.</span></span>

<span data-ttu-id="16924-112">Преобразования определяются с помощью двух `Func` деревьев выражений: одно из `ModelClrType` в `ProviderClrType` , а другое — от `ProviderClrType` до `ModelClrType` .</span><span class="sxs-lookup"><span data-stu-id="16924-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="16924-113">Деревья выражений используются, чтобы их можно было скомпилировать в код доступа к базе данных для эффективного преобразования.</span><span class="sxs-lookup"><span data-stu-id="16924-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="16924-114">Для сложных преобразований дерево выражений может быть простым вызовом метода, который выполняет преобразование.</span><span class="sxs-lookup"><span data-stu-id="16924-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="16924-115">Настройка преобразователя значений</span><span class="sxs-lookup"><span data-stu-id="16924-115">Configuring a value converter</span></span>

<span data-ttu-id="16924-116">Преобразования значений определяются для свойств в `OnModelCreating` `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="16924-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="16924-117">Например, рассмотрим перечисление и тип сущности, определенный как:</span><span class="sxs-lookup"><span data-stu-id="16924-117">For example, consider an enum and entity type defined as:</span></span>

```csharp
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
```

<span data-ttu-id="16924-118">Затем можно определить преобразования в `OnModelCreating` для хранения значений перечисления в виде строк (например, "донкэй", "Муле",...) в базе данных:</span><span class="sxs-lookup"><span data-stu-id="16924-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```

> [!NOTE]
> <span data-ttu-id="16924-119">`null`Значение никогда не будет передано преобразователю значений.</span><span class="sxs-lookup"><span data-stu-id="16924-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="16924-120">Это упрощает реализацию преобразований и позволяет им совместно использовать свойства, допускающие значения NULL и не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="16924-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="16924-121">Класс ValueConverter</span><span class="sxs-lookup"><span data-stu-id="16924-121">The ValueConverter class</span></span>

<span data-ttu-id="16924-122">Вызов `HasConversion` , как показано выше, создаст `ValueConverter` экземпляр и установит его в свойстве.</span><span class="sxs-lookup"><span data-stu-id="16924-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="16924-123">`ValueConverter`Вместо этого можно создать явно.</span><span class="sxs-lookup"><span data-stu-id="16924-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="16924-124">Например.</span><span class="sxs-lookup"><span data-stu-id="16924-124">For example:</span></span>

```csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="16924-125">Это может быть полезно, если несколько свойств используют одно и то же преобразование.</span><span class="sxs-lookup"><span data-stu-id="16924-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]
> <span data-ttu-id="16924-126">В настоящее время нет способа указать в одном месте, что каждое свойство данного типа должно использовать один и тот же преобразователь значений.</span><span class="sxs-lookup"><span data-stu-id="16924-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="16924-127">Этот компонент будет рассмотрен в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="16924-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="16924-128">Встроенные преобразователи</span><span class="sxs-lookup"><span data-stu-id="16924-128">Built-in converters</span></span>

<span data-ttu-id="16924-129">EF Core поставляется с набором предварительно определенных `ValueConverter` классов, найденных в `Microsoft.EntityFrameworkCore.Storage.ValueConversion` пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="16924-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="16924-130">Эти особые значения приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="16924-130">These are:</span></span>

* <span data-ttu-id="16924-131">`BoolToZeroOneConverter` -Bool — ноль и один</span><span class="sxs-lookup"><span data-stu-id="16924-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="16924-132">`BoolToStringConverter` -Bool для строк, таких как "Y" и "N"</span><span class="sxs-lookup"><span data-stu-id="16924-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="16924-133">`BoolToTwoValuesConverter` -Bool для любых двух значений</span><span class="sxs-lookup"><span data-stu-id="16924-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="16924-134">`BytesToStringConverter` — Массив байтов в строку в кодировке Base64</span><span class="sxs-lookup"><span data-stu-id="16924-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="16924-135">`CastingConverter` — Преобразования, для которых требуется только приведение типов</span><span class="sxs-lookup"><span data-stu-id="16924-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="16924-136">`CharToStringConverter` -Char — строка одиночного символа</span><span class="sxs-lookup"><span data-stu-id="16924-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="16924-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset в двоично-закодированное 64-разрядное значение</span><span class="sxs-lookup"><span data-stu-id="16924-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="16924-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset в массив байтов</span><span class="sxs-lookup"><span data-stu-id="16924-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="16924-139">`DateTimeOffsetToStringConverter` -DateTimeOffset в строку</span><span class="sxs-lookup"><span data-stu-id="16924-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="16924-140">`DateTimeToBinaryConverter` -DateTime в 64-разрядное значение, включая DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="16924-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="16924-141">`DateTimeToStringConverter` -DateTime в строку</span><span class="sxs-lookup"><span data-stu-id="16924-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="16924-142">`DateTimeToTicksConverter` -DateTime — такты</span><span class="sxs-lookup"><span data-stu-id="16924-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="16924-143">`EnumToNumberConverter` -Перечислить в базовое число</span><span class="sxs-lookup"><span data-stu-id="16924-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="16924-144">`EnumToStringConverter` -Enum в строку</span><span class="sxs-lookup"><span data-stu-id="16924-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="16924-145">`GuidToBytesConverter` -GUID в массив байтов</span><span class="sxs-lookup"><span data-stu-id="16924-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="16924-146">`GuidToStringConverter` -GUID в строку</span><span class="sxs-lookup"><span data-stu-id="16924-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="16924-147">`NumberToBytesConverter` — Любое числовое значение массива байтов</span><span class="sxs-lookup"><span data-stu-id="16924-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="16924-148">`NumberToStringConverter` — Любое числовое значение в строке</span><span class="sxs-lookup"><span data-stu-id="16924-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="16924-149">`StringToBytesConverter` — Строка — UTF8-байт</span><span class="sxs-lookup"><span data-stu-id="16924-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="16924-150">`TimeSpanToStringConverter` -TimeSpan до строки</span><span class="sxs-lookup"><span data-stu-id="16924-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="16924-151">`TimeSpanToTicksConverter` -TimeSpan — такты</span><span class="sxs-lookup"><span data-stu-id="16924-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="16924-152">Обратите внимание, что `EnumToStringConverter` включен в этот список.</span><span class="sxs-lookup"><span data-stu-id="16924-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="16924-153">Это означает, что нет необходимости явно указывать преобразование, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="16924-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="16924-154">Вместо этого просто используйте встроенный преобразователь:</span><span class="sxs-lookup"><span data-stu-id="16924-154">Instead, just use the built-in converter:</span></span>

```csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="16924-155">Обратите внимание, что все встроенные преобразователи не имеют состояния, поэтому один экземпляр может быть безопасно совместно использоваться несколькими свойствами.</span><span class="sxs-lookup"><span data-stu-id="16924-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="16924-156">Предварительно определенные преобразования</span><span class="sxs-lookup"><span data-stu-id="16924-156">Pre-defined conversions</span></span>

<span data-ttu-id="16924-157">Для распространенных преобразований, для которых существует встроенный преобразователь, нет необходимости явно указывать преобразователь.</span><span class="sxs-lookup"><span data-stu-id="16924-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="16924-158">Вместо этого следует просто настроить тип поставщика, который должен использоваться, и EF будет автоматически использовать соответствующий встроенный преобразователь.</span><span class="sxs-lookup"><span data-stu-id="16924-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="16924-159">Перечисление для преобразования строк используется как пример выше, но EF автоматически выполняет это действие, если тип поставщика настроен:</span><span class="sxs-lookup"><span data-stu-id="16924-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

```csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="16924-160">То же самое можно добиться, явно указав тип столбца.</span><span class="sxs-lookup"><span data-stu-id="16924-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="16924-161">Например, если тип сущности определен следующим образом:</span><span class="sxs-lookup"><span data-stu-id="16924-161">For example, if the entity type is defined like so:</span></span>

```csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="16924-162">Затем значения перечисления будут сохранены в базе данных в виде строк без дополнительной настройки в `OnModelCreating` .</span><span class="sxs-lookup"><span data-stu-id="16924-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="16924-163">Ограничения</span><span class="sxs-lookup"><span data-stu-id="16924-163">Limitations</span></span>

<span data-ttu-id="16924-164">Существует несколько известных текущих ограничений системы преобразования значений:</span><span class="sxs-lookup"><span data-stu-id="16924-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="16924-165">Как отмечалось выше, `null` невозможно преобразовать.</span><span class="sxs-lookup"><span data-stu-id="16924-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="16924-166">В настоящее время невозможно распределить преобразование одного свойства в несколько столбцов или наоборот.</span><span class="sxs-lookup"><span data-stu-id="16924-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="16924-167">Использование преобразований значений может повлиять на способность EF Core перевести выражения в SQL.</span><span class="sxs-lookup"><span data-stu-id="16924-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="16924-168">В таких случаях будет зарегистрировано предупреждение.</span><span class="sxs-lookup"><span data-stu-id="16924-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="16924-169">Удаление этих ограничений рассматривается в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="16924-169">Removal of these limitations is being considered for a future release.</span></span>
