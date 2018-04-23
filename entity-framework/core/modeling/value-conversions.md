---
title: Преобразование значения - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 329d2757059462468ca30772d37789343c03ba7b
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="value-conversions"></a>Преобразования значений

> [!NOTE]  
> Этот компонент впервые появился в EF Core 2.1.

Преобразователи значений позволяет значениям свойств преобразования при чтении или записи в базу данных. Это преобразование может быть одно значение с другим того же типа (например, шифрованной строки) или значение одного типа на значение другого типа (например, преобразование значения перечисления в и из строки в базе данных.)

## <a name="fundamentals"></a>Основы

Преобразователи значений указываются в виде `ModelClrType` и `ProviderClrType`. Тип модели является типом .NET свойства в типе сущности. Тип поставщика — тип .NET, подразумевается поставщик базы данных. Например, чтобы сохранить перечисления строк в базе данных, тип модели является тип перечисления, и поставщика тип `String`. Эти два типа могут быть одинаковыми.

Преобразования определяются с использованием двух `Func` деревьев выражений: один из `ModelClrType` для `ProviderClrType` , а другой из `ProviderClrType` для `ModelClrType`. Деревья выражений используются, чтобы их можно скомпилировать в код доступа базы данных для эффективного преобразования. Для сложных преобразований дерева выражения может быть простой вызов метода, который выполняет преобразование.

## <a name="configuring-a-value-converter"></a>Настройка преобразователя значений

Преобразование значения для свойств в OnModelCreating вашей DbContext определено. Например рассмотрим перечисление и сущности типа, определенного как:
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
Затем преобразования могут быть определены в OnModelCreating для хранения значений enum как строки (например) «Спецов», «Mule»,...) в базе данных:
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
> Объект `null` значение никогда не передается в преобразователь значений. Это облегчает реализацию преобразований и позволяет им быть общим для свойства допускает значения NULL и не допускает значения NULL.

## <a name="the-valueconverter-class"></a>Класс ValueConverter

Вызов `HasConversion` как показано выше, будет создан `ValueConverter` экземпляра и задать его свойству. `ValueConverter` Вместо могут создаваться явно. Пример:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Это полезно в том случае, если несколько свойств используют то же самое преобразование.

> [!NOTE]  
> Сейчас невозможно указать в одном месте, что каждое свойство данного типа должны использовать один и тот же преобразователь значения. Этот компонент будет считаться в будущих выпусках.

## <a name="built-in-converters"></a>Встроенные конвертеры

Предварительно определенные EF Core поставляется с набором `ValueConverter` классы, в `Microsoft.EntityFrameworkCore.Storage.Converters` пространства имен. Эти особые значения приведены ниже.
* `BoolToZeroOneConverter` -Bool для нуля до единицы
* `BoolToStringConverter` -Bool строки, такие как «Y» и «N»
* `BoolToTwoValuesConverter` -Bool любые два значения
* `BytesToStringConverter` -Байтовый массив для строки в кодировке Base64
* `CastingConverter` -Преобразования, требующих приведение c#
* `CharToStringConverter` -Char для одной символьной строки
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset в двоичной кодировке 64-разрядное значение
* `DateTimeOffsetToBytesConverter` -DateTimeOffset байтовый массив
* `DateTimeOffsetToStringConverter` -DateTimeOffset строку
* `DateTimeToBinaryConverter` -DateTime 64-разрядное значение, включая DateTimeKind
* `DateTimeToStringConverter` -DateTime в строку
* `DateTimeToTicksConverter` -DateTime меткам
* `EnumToNumberConverter` -Enum базовый номер
* `EnumToStringConverter` -Enum строку
* `GuidToBytesConverter` -Guid байтовый массив
* `GuidToStringConverter` -Guid строку
* `NumberToBytesConverter` -Любое числовое значение массива байтов
* `NumberToStringConverter` -Любое числовое значение в строку
* `StringToBytesConverter` -Строка UTF-8 байты
* `TimeSpanToStringConverter` -TimeSpan в строку
* `TimeSpanToTicksConverter` -TimeSpan тактов

Обратите внимание, что `EnumToStringConverter` включаются в этот список. Это означает, что нет необходимости указывать преобразование явным образом, как показано выше. Вместо этого используйте встроенные преобразователя:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Обратите внимание, что встроенные преобразователи типов не имеют состояния таким образом один экземпляр может безопасно совместно использоваться несколькими свойствами.

## <a name="pre-defined-conversions"></a>Предопределенные преобразования

Для общих преобразований, для которых существует встроенный преобразователь нет необходимости явно указывать преобразователь. Вместо этого просто настроить подходящего поставщика можно использовать и EF будут автоматически использовать соответствующий конвертер сборки. Перечисление для преобразования строк используются как в примере выше, но EF фактически делает это автоматически, если настроен тип поставщика:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
То же самое может осуществляться путем явного указания типа столбца. Например, если определяется тип сущности, например так:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Значения перечисления будут сохраняться как строки в базе данных без дополнительной настройки в OnModelCreating.

## <a name="limitations"></a>Ограничения

Существует несколько ограничений известных текущей системы преобразования значения.
* Как отмечалось выше, `null` не может быть преобразован.
* В настоящее время нет возможности для распределения преобразование одного свойства для нескольких столбцов или наоборот.
* Использование преобразования значений может повлиять на возможность EF Core перевод выражений для SQL. В таких случаях будет регистрироваться предупреждения.
Удаление этих ограничений считается в будущих выпусках.
