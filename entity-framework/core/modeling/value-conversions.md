---
title: Преобразования значений — EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 5bfb6111ac450db91f3f1a7074a924a1c8400ce7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949096"
---
# <a name="value-conversions"></a><span data-ttu-id="d98b5-102">Преобразования значений</span><span class="sxs-lookup"><span data-stu-id="d98b5-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="d98b5-103">Это новая возможность в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d98b5-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="d98b5-104">Преобразователи значений позволяет значениям свойств преобразования при чтении или записи в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d98b5-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="d98b5-105">Это преобразование может быть от одного значения к другому того же типа (например, шифрование строки) или из значения одного типа в значение другого типа (например, преобразование перечисления значения строки в базе данных и обратно.)</span><span class="sxs-lookup"><span data-stu-id="d98b5-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="d98b5-106">Основы</span><span class="sxs-lookup"><span data-stu-id="d98b5-106">Fundamentals</span></span>

<span data-ttu-id="d98b5-107">Преобразователи значений указываются в виде `ModelClrType` и `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="d98b5-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="d98b5-108">Тип модели является типом .NET свойства в типе сущности.</span><span class="sxs-lookup"><span data-stu-id="d98b5-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="d98b5-109">Тип поставщика является типом .NET, подразумевается поставщик базы данных.</span><span class="sxs-lookup"><span data-stu-id="d98b5-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="d98b5-110">Например, чтобы сохранить перечислений в виде строк в базе данных, тип модели — тип перечисления и поставщик является `String`.</span><span class="sxs-lookup"><span data-stu-id="d98b5-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="d98b5-111">Эти два типа могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d98b5-111">These two types can be the same.</span></span>

<span data-ttu-id="d98b5-112">Преобразования определяются с использованием двух `Func` деревьев выражений: один из `ModelClrType` для `ProviderClrType` , а другой из `ProviderClrType` для `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="d98b5-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="d98b5-113">Деревья выражений используются, чтобы их можно скомпилировать в код доступа базы данных для эффективного преобразования.</span><span class="sxs-lookup"><span data-stu-id="d98b5-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="d98b5-114">Для выполнения сложных преобразований дерева выражения может быть простой вызов метода, который выполняет преобразование.</span><span class="sxs-lookup"><span data-stu-id="d98b5-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="d98b5-115">Настройка преобразователя значений</span><span class="sxs-lookup"><span data-stu-id="d98b5-115">Configuring a value converter</span></span>

<span data-ttu-id="d98b5-116">Преобразования значений для свойств в OnModelCreating DbContext определено.</span><span class="sxs-lookup"><span data-stu-id="d98b5-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="d98b5-117">Например рассмотрим enum и сущности типа, определенного как:</span><span class="sxs-lookup"><span data-stu-id="d98b5-117">For example, consider an enum and entity type defined as:</span></span>
```Csharp
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
<span data-ttu-id="d98b5-118">Затем преобразования можно определять в OnModelCreating, чтобы сохранять значения перечисления в виде строк (например, «Спецов», «Mule»,...) в базе данных:</span><span class="sxs-lookup"><span data-stu-id="d98b5-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
```Csharp
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
> <span data-ttu-id="d98b5-119">Объект `null` значение никогда не будут передаваться преобразователь значений.</span><span class="sxs-lookup"><span data-stu-id="d98b5-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="d98b5-120">Это облегчает реализацию преобразования и позволяет им быть общим для свойства допускает значения NULL и не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="d98b5-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="d98b5-121">Класс ValueConverter</span><span class="sxs-lookup"><span data-stu-id="d98b5-121">The ValueConverter class</span></span>

<span data-ttu-id="d98b5-122">Вызов `HasConversion` как показано выше, создаст `ValueConverter` экземпляра и задать его свойству.</span><span class="sxs-lookup"><span data-stu-id="d98b5-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="d98b5-123">`ValueConverter` Вместо этого можно явным образом создать.</span><span class="sxs-lookup"><span data-stu-id="d98b5-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="d98b5-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="d98b5-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="d98b5-125">Это может быть полезно, когда несколько свойств используйте то же самое преобразование.</span><span class="sxs-lookup"><span data-stu-id="d98b5-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="d98b5-126">В настоящее время нет способа указать в одном месте, что каждое свойство данного типа должны использовать один и тот же преобразователь значения.</span><span class="sxs-lookup"><span data-stu-id="d98b5-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="d98b5-127">Эта функция будет рассматриваться в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="d98b5-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="d98b5-128">Встроенные конвертеры</span><span class="sxs-lookup"><span data-stu-id="d98b5-128">Built-in converters</span></span>

<span data-ttu-id="d98b5-129">EF Core поставляется с набором предварительно определенных `ValueConverter` классами, имеющимися в `Microsoft.EntityFrameworkCore.Storage.ValueConversion` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d98b5-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="d98b5-130">Эти особые значения приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="d98b5-130">These are:</span></span>
* <span data-ttu-id="d98b5-131">`BoolToZeroOneConverter` -Bool в 0 и 1</span><span class="sxs-lookup"><span data-stu-id="d98b5-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="d98b5-132">`BoolToStringConverter` -Bool для строк, таких как «Y» и «N»</span><span class="sxs-lookup"><span data-stu-id="d98b5-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="d98b5-133">`BoolToTwoValuesConverter` -Bool любые два значения</span><span class="sxs-lookup"><span data-stu-id="d98b5-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="d98b5-134">`BytesToStringConverter` -Массив байтов строку в кодировке Base64</span><span class="sxs-lookup"><span data-stu-id="d98b5-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="d98b5-135">`CastingConverter` -Преобразования, которые требуется только приведение c#</span><span class="sxs-lookup"><span data-stu-id="d98b5-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="d98b5-136">`CharToStringConverter` -Char в одну символьную строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="d98b5-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset в двоичной кодировке 64-разрядное значение</span><span class="sxs-lookup"><span data-stu-id="d98b5-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="d98b5-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset байтовый массив</span><span class="sxs-lookup"><span data-stu-id="d98b5-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="d98b5-139">`DateTimeOffsetToStringConverter` -DateTimeOffset строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="d98b5-140">`DateTimeToBinaryConverter` -DateTime на 64-разрядное значение, включая DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="d98b5-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="d98b5-141">`DateTimeToStringConverter` -DateTime в строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="d98b5-142">`DateTimeToTicksConverter` -Дата и время в тактах</span><span class="sxs-lookup"><span data-stu-id="d98b5-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="d98b5-143">`EnumToNumberConverter` -Enum основной номер</span><span class="sxs-lookup"><span data-stu-id="d98b5-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="d98b5-144">`EnumToStringConverter` -Enum строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="d98b5-145">`GuidToBytesConverter` -Guid байтовый массив</span><span class="sxs-lookup"><span data-stu-id="d98b5-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="d98b5-146">`GuidToStringConverter` -Guid строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="d98b5-147">`NumberToBytesConverter` — Любое числовое значение, массив байтов</span><span class="sxs-lookup"><span data-stu-id="d98b5-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="d98b5-148">`NumberToStringConverter` — Любое числовое значение в строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="d98b5-149">`StringToBytesConverter` -Строка для UTF8 байты</span><span class="sxs-lookup"><span data-stu-id="d98b5-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="d98b5-150">`TimeSpanToStringConverter` -TimeSpan в строку</span><span class="sxs-lookup"><span data-stu-id="d98b5-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="d98b5-151">`TimeSpanToTicksConverter` -TimeSpan меткам</span><span class="sxs-lookup"><span data-stu-id="d98b5-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="d98b5-152">Обратите внимание, что `EnumToStringConverter` включается в этот список.</span><span class="sxs-lookup"><span data-stu-id="d98b5-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="d98b5-153">Это означает, что нет необходимости для преобразования явно указать, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="d98b5-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="d98b5-154">Вместо этого используйте встроенный преобразователь:</span><span class="sxs-lookup"><span data-stu-id="d98b5-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="d98b5-155">Обратите внимание, что все встроенные преобразователей не имеют состояния, и поэтому один экземпляр можно безопасно использовать их совместно с несколькими свойствами.</span><span class="sxs-lookup"><span data-stu-id="d98b5-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="d98b5-156">Предопределенные преобразования</span><span class="sxs-lookup"><span data-stu-id="d98b5-156">Pre-defined conversions</span></span>

<span data-ttu-id="d98b5-157">Для общих преобразований, для которых существует встроенный преобразователь нет необходимости явно указать преобразователь.</span><span class="sxs-lookup"><span data-stu-id="d98b5-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="d98b5-158">Вместо этого достаточно настроить следует использовать тип поставщика и EF будет автоматически использовать конвертер в сборки.</span><span class="sxs-lookup"><span data-stu-id="d98b5-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="d98b5-159">Перечисление для преобразования строк используются как в примере выше, но EF фактически сделает это автоматически при настройке тип поставщика:</span><span class="sxs-lookup"><span data-stu-id="d98b5-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="d98b5-160">То же самое достигается путем явного указания типа столбца.</span><span class="sxs-lookup"><span data-stu-id="d98b5-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="d98b5-161">Например, если определен тип сущности, например так:</span><span class="sxs-lookup"><span data-stu-id="d98b5-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="d98b5-162">Затем значения перечисления сохраняется в виде строк в базе данных без дополнительной настройки в OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="d98b5-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="d98b5-163">Ограничения</span><span class="sxs-lookup"><span data-stu-id="d98b5-163">Limitations</span></span>

<span data-ttu-id="d98b5-164">Существует несколько известных ограничений системы преобразования значения.</span><span class="sxs-lookup"><span data-stu-id="d98b5-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="d98b5-165">Как отмечалось выше, `null` не может быть преобразован.</span><span class="sxs-lookup"><span data-stu-id="d98b5-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="d98b5-166">В настоящее время нет способа распространения преобразование одного из свойств для нескольких столбцов или наоборот.</span><span class="sxs-lookup"><span data-stu-id="d98b5-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="d98b5-167">Использование преобразования значений может повлиять на возможность преобразования выражений SQL EF Core.</span><span class="sxs-lookup"><span data-stu-id="d98b5-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="d98b5-168">В таких случаях будет зарегистрировано предупреждение.</span><span class="sxs-lookup"><span data-stu-id="d98b5-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="d98b5-169">Удаление этих ограничений считается в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="d98b5-169">Removal of these limitations is being considered for a future release.</span></span>
