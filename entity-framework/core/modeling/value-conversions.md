---
title: Преобразования значений — EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 2a1956221ecc920feba796e4d95cc97259e89c53
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152514"
---
# <a name="value-conversions"></a>Преобразования значений

> [!NOTE]  
> Это новая возможность в EF Core 2.1.

Преобразователи значений позволяет значениям свойств преобразования при чтении или записи в базу данных. Это преобразование может быть от одного значения к другому того же типа (например, шифрование строки) или из значения одного типа в значение другого типа (например, преобразование перечисления значения строки в базе данных и обратно.)

## <a name="fundamentals"></a>Основы

Преобразователи значений указываются в виде `ModelClrType` и `ProviderClrType`. Тип модели является типом .NET свойства в типе сущности. Тип поставщика является типом .NET, подразумевается поставщик базы данных. Например, чтобы сохранить перечислений в виде строк в базе данных, тип модели — тип перечисления и поставщик является `String`. Эти два типа могут совпадать.

Преобразования определяются с использованием двух `Func` деревьев выражений: один из `ModelClrType` для `ProviderClrType` , а другой из `ProviderClrType` для `ModelClrType`. Деревья выражений используются, чтобы их можно скомпилировать в код доступа базы данных для эффективного преобразования. Для выполнения сложных преобразований дерева выражения может быть простой вызов метода, который выполняет преобразование.

## <a name="configuring-a-value-converter"></a>Настройка преобразователя значений

Преобразования значений определяются в свойствах в `OnModelCreating` из вашей `DbContext`. Например рассмотрим enum и сущности типа, определенного как:
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
Затем можно определить преобразования в `OnModelCreating` для хранения значений enum как строки (например, «Спецов», «Mule»,...) в базе данных:
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
> Объект `null` значение никогда не будут передаваться преобразователь значений. Это облегчает реализацию преобразования и позволяет им быть общим для свойства допускает значения NULL и не допускающие значения NULL.

## <a name="the-valueconverter-class"></a>Класс ValueConverter

Вызов `HasConversion` как показано выше, создаст `ValueConverter` экземпляра и задать его свойству. `ValueConverter` Вместо этого можно явным образом создать. Пример:
``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Это может быть полезно, когда несколько свойств используйте то же самое преобразование.

> [!NOTE]  
> В настоящее время нет способа указать в одном месте, что каждое свойство данного типа должны использовать один и тот же преобразователь значения. Эта функция будет рассматриваться в будущих выпусках.

## <a name="built-in-converters"></a>Встроенные конвертеры

EF Core поставляется с набором предварительно определенных `ValueConverter` классами, имеющимися в `Microsoft.EntityFrameworkCore.Storage.ValueConversion` пространства имен. Эти особые значения приведены ниже.
* `BoolToZeroOneConverter` -Bool в 0 и 1
* `BoolToStringConverter` -Bool для строк, таких как «Y» и «N»
* `BoolToTwoValuesConverter` -Bool любые два значения
* `BytesToStringConverter` -Массив байтов строку в кодировке Base64
* `CastingConverter` -Преобразования, которые требуется только приведение типа
* `CharToStringConverter` -Char в одну символьную строку
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset в двоичной кодировке 64-разрядное значение
* `DateTimeOffsetToBytesConverter` -DateTimeOffset байтовый массив
* `DateTimeOffsetToStringConverter` -DateTimeOffset строку
* `DateTimeToBinaryConverter` -DateTime на 64-разрядное значение, включая DateTimeKind
* `DateTimeToStringConverter` -DateTime в строку
* `DateTimeToTicksConverter` -Дата и время в тактах
* `EnumToNumberConverter` -Enum основной номер
* `EnumToStringConverter` -Enum строку
* `GuidToBytesConverter` -Guid байтовый массив
* `GuidToStringConverter` -Guid строку
* `NumberToBytesConverter` — Любое числовое значение, массив байтов
* `NumberToStringConverter` — Любое числовое значение в строку
* `StringToBytesConverter` -Строка для UTF8 байты
* `TimeSpanToStringConverter` -TimeSpan в строку
* `TimeSpanToTicksConverter` -TimeSpan меткам

Обратите внимание, что `EnumToStringConverter` включается в этот список. Это означает, что нет необходимости для преобразования явно указать, как показано выше. Вместо этого используйте встроенный преобразователь:
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Обратите внимание, что все встроенные преобразователей не имеют состояния, и поэтому один экземпляр можно безопасно использовать их совместно с несколькими свойствами.

## <a name="pre-defined-conversions"></a>Предопределенные преобразования

Для общих преобразований, для которых существует встроенный преобразователь нет необходимости явно указать преобразователь. Вместо этого достаточно настроить следует использовать тип поставщика и EF будет автоматически использовать соответствующий встроенный преобразователь. Перечисление для преобразования строк используются как в примере выше, но EF фактически сделает это автоматически при настройке тип поставщика:
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
То же самое достигается путем явного указания типа столбца. Например, если определен тип сущности, например так:
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Затем значения перечисления будет сохранен как строки в базе данных без дополнительной настройки в `OnModelCreating`.

## <a name="limitations"></a>Ограничения

Существует несколько известных ограничений системы преобразования значения.
* Как отмечалось выше, `null` не может быть преобразован.
* В настоящее время нет способа распространения преобразование одного из свойств для нескольких столбцов или наоборот.
* Использование преобразования значений может повлиять на возможность преобразования выражений SQL EF Core. В таких случаях будет зарегистрировано предупреждение.
Удаление этих ограничений считается в будущих выпусках.
