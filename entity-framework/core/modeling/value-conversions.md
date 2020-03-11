---
title: Преобразования значений — EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414557"
---
# <a name="value-conversions"></a>Преобразования значений

> [!NOTE]  
> Это новая возможность в EF Core 2.1.

Преобразователи значений позволяют преобразовывать значения свойств при чтении из базы данных или записи в нее. Это преобразование может быть из одного значения другого типа (например, для шифрования строк) или из значения одного типа в значение другого типа (например, преобразование перечисляемых значений в строки базы данных и из них).

## <a name="fundamentals"></a>Основные сведения

Преобразователи значений указываются в терминах `ModelClrType` и `ProviderClrType`. Тип модели — это тип .NET свойства в типе сущности. Тип поставщика — это тип .NET, понятный поставщику базы данных. Например, чтобы сохранить перечисления в виде строк в базе данных, тип модели является типом перечисления, а тип поставщика — `String`. Эти два типа могут быть одинаковыми.

Преобразования определяются с помощью двух деревьев выражений `Func`: одно из `ModelClrType` в `ProviderClrType` и другое из `ProviderClrType` в `ModelClrType`. Деревья выражений используются, чтобы их можно было скомпилировать в код доступа к базе данных для эффективного преобразования. Для сложных преобразований дерево выражений может быть простым вызовом метода, который выполняет преобразование.

## <a name="configuring-a-value-converter"></a>Настройка преобразователя значений

Преобразования значений определяются в свойствах `OnModelCreating` `DbContext`. Например, рассмотрим перечисление и тип сущности, определенный как:

``` csharp
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

Затем можно определить преобразования в `OnModelCreating` для хранения значений перечисления в виде строк (например, "Донкэй", "Муле",...) в базе данных:

``` csharp
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
> Значение `null` никогда не будет передано преобразователю значений. Это упрощает реализацию преобразований и позволяет им совместно использовать свойства, допускающие значения NULL и не допускающие значения NULL.

## <a name="the-valueconverter-class"></a>Класс ValueConverter

Вызов `HasConversion`, как показано выше, создаст экземпляр `ValueConverter` и установит его в свойстве. Вместо этого `ValueConverter` можно создать явным образом. Пример:

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Это может быть полезно, если несколько свойств используют одно и то же преобразование.

> [!NOTE]  
> В настоящее время нет способа указать в одном месте, что каждое свойство данного типа должно использовать один и тот же преобразователь значений. Этот компонент будет рассмотрен в будущем выпуске.

## <a name="built-in-converters"></a>Встроенные преобразователи

EF Core поставляется с набором предварительно определенных классов `ValueConverter`, которые находятся в пространстве имен `Microsoft.EntityFrameworkCore.Storage.ValueConversion`. а именно:

* `BoolToZeroOneConverter`-bool равно нулю и одному
* `BoolToStringConverter`-bool для строк, таких как "Y" и "N"
* `BoolToTwoValuesConverter`-bool для любых двух значений
* массив `BytesToStringConverter` байт в строку в кодировке Base64
* `CastingConverter`-преобразования, для которых требуется только приведение типов
* `CharToStringConverter`-char в строку одиночных символов
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset в двоично-закодированное 64-разрядное значение
* `DateTimeOffsetToBytesConverter`-DateTimeOffset в массив байтов
* `DateTimeOffsetToStringConverter`-DateTimeOffset в строку
* `DateTimeToBinaryConverter`-DateTime в 64-разрядное значение, включая DateTimeKind
* `DateTimeToStringConverter`-DateTime в строку
* `DateTimeToTicksConverter`-DateTime в такты
* Перечисление `EnumToNumberConverter` в базовое число
* `EnumToStringConverter` перечисление в строку
* `GuidToBytesConverter`-GUID массива байтов
* `GuidToStringConverter`-GUID в строку
* `NumberToBytesConverter`-любое числовое значение в массив байтов
* `NumberToStringConverter`-любое числовое значение в строку
* `StringToBytesConverter` — строка в UTF8-байт
* `TimeSpanToStringConverter` TimeSpan в строку
* `TimeSpanToTicksConverter` с интервалом до тактов

Обратите внимание, что в этот список включен `EnumToStringConverter`. Это означает, что нет необходимости явно указывать преобразование, как показано выше. Вместо этого просто используйте встроенный преобразователь:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Обратите внимание, что все встроенные преобразователи не имеют состояния, поэтому один экземпляр может быть безопасно совместно использоваться несколькими свойствами.

## <a name="pre-defined-conversions"></a>Предварительно определенные преобразования

Для распространенных преобразований, для которых существует встроенный преобразователь, нет необходимости явно указывать преобразователь. Вместо этого следует просто настроить тип поставщика, который должен использоваться, и EF будет автоматически использовать соответствующий встроенный преобразователь. Перечисление для преобразования строк используется как пример выше, но EF автоматически выполняет это действие, если тип поставщика настроен:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

То же самое можно добиться, явно указав тип столбца. Например, если тип сущности определен следующим образом:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

Затем значения перечисления будут сохранены в базе данных в виде строк без дополнительной настройки в `OnModelCreating`.

## <a name="limitations"></a>Ограничения

Существует несколько известных текущих ограничений системы преобразования значений:

* Как отмечалось выше, `null` не может быть преобразован.
* В настоящее время невозможно распределить преобразование одного свойства в несколько столбцов или наоборот.
* Использование преобразований значений может повлиять на способность EF Core перевести выражения в SQL. В таких случаях будет зарегистрировано предупреждение.
Удаление этих ограничений рассматривается в будущем выпуске.
