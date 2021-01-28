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
# <a name="value-conversions"></a>Преобразования значений

Преобразователи значений позволяют преобразовывать значения свойств при чтении из базы данных или записи в нее. Это преобразование может быть из одного значения другого типа (например, для шифрования строк) или из значения одного типа в значение другого типа (например, преобразование перечисляемых значений в строки базы данных и из них).

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).

## <a name="overview"></a>Обзор

Преобразователи значений указываются в терминах `ModelClrType` и `ProviderClrType` . Тип модели — это тип .NET свойства в типе сущности. Тип поставщика — это тип .NET, понятный поставщику базы данных. Например, чтобы сохранить перечисления в виде строк в базе данных, тип модели является типом перечисления, а тип поставщика — `String` . Эти два типа могут быть одинаковыми.

Преобразования определяются с помощью двух `Func` деревьев выражений: одно из `ModelClrType` в `ProviderClrType` , а другое — от `ProviderClrType` до `ModelClrType` . Деревья выражений используются, чтобы их можно было скомпилировать в делегат доступа к базе данных для эффективного преобразования. Дерево выражения может содержать простой вызов метода преобразования для сложных преобразований.

> [!NOTE]
> Для свойства, которое было настроено для преобразования значений, также может потребоваться указать <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> . Дополнительные сведения см. в примерах ниже и документации по [компараторам значений](xref:core/modeling/value-comparers) .

## <a name="configuring-a-value-converter"></a>Настройка преобразователя значений

Преобразования значений настраиваются в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType> . Например, рассмотрим перечисление и тип сущности, определенный как:

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

Преобразования могут быть настроены в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> для хранения значений перечисления в виде строк, таких как "донкэй", "Муле" и т. д. в базе данных. необходимо просто предоставить одну функцию, преобразующую из в `ModelClrType` `ProviderClrType` , и другую для обратного преобразования:

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
> `null`Значение никогда не будет передано преобразователю значений. Значение NULL в столбце базы данных всегда равно NULL в экземпляре сущности и наоборот. Это упрощает реализацию преобразований и позволяет им совместно использовать свойства, допускающие значения NULL и не допускающие значения NULL. Дополнительные сведения см. в [статье о проблемах #13850 GitHub](https://github.com/dotnet/efcore/issues/13850) .

## <a name="pre-defined-conversions"></a>Предварительно определенные преобразования

EF Core содержит множество предварительно определенных преобразований, которые избегают необходимость ручной записи функций преобразования. Вместо этого EF Core выберет преобразование для использования на основе типа свойства в модели и запрошенного типа поставщика базы данных.

Например, перечисление в преобразования строк используется как пример выше, но EF Core на самом деле сделает это автоматически, если тип поставщика настроен так, как `string` с помощью универсального типа <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.HasConversion%2A> :

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

То же самое можно достичь, явно указав тип столбца базы данных. Например, если тип сущности определен следующим образом:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

<!--
        public class Rider2
        {
            public int Id { get; set; }

            [Column(TypeName = "nvarchar(24)")]
            public EquineBeast Mount { get; set; }
        }
-->
[!code-csharp[ConversionByDatabaseType](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByDatabaseType)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

<!--
                modelBuilder
                    .Entity<Rider2>()
                    .Property(e => e.Mount)
                    .HasColumnType("nvarchar(24)");
-->
[!code-csharp[ConversionByDatabaseTypeFluent](../../../samples/core/Modeling/ValueConversions/EnumToStringConversions.cs?name=ConversionByDatabaseTypeFluent)]

***

Затем значения перечисления будут сохранены в базе данных в виде строк без дополнительной настройки в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> .

## <a name="the-valueconverter-class"></a>Класс ValueConverter

Вызов <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.HasConversion%2A> , как показано выше, создаст <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ValueConverter%602> экземпляр и установит его в свойстве. `ValueConverter`Вместо этого можно создать явно. Пример:

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

Это может быть полезно, если несколько свойств используют одно и то же преобразование.

## <a name="built-in-converters"></a>Встроенные преобразователи

Как упоминалось выше, EF Core поставляется с набором предварительно определенных <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ValueConverter%602> классов, найденных в <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion> пространстве имен. Во многих случаях EF выберет соответствующий встроенный преобразователь, основанный на типе свойства в модели, и тип, запрошенный в базе данных, как показано выше для перечислений. Например, использование `.HasConversion<int>()` в `bool` свойстве приводит к тому, что EF Core преобразует логические значения в числовые ноль и одно значение:

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

Это функционально то же самое, что и создание экземпляра встроенного <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToZeroOneConverter%601> и его явное задание:

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

В следующей таблице перечислены распространенные предварительно определенные преобразования из типов модели или свойства в типы поставщиков баз данных. В таблице `any_numeric_type` означает один из способов `int` :,,,, `short` `long` `byte` `uint` , `ushort` , `ulong` , `sbyte` ,,, `char` `decimal` `float` или `double` .

| Тип модели или свойства | Тип поставщика или базы данных | Преобразование                                                | Использование
|:--------------------|------------------------|-----------------------------------------------------------|------
| bool                | any_numeric_type       | False/true в 0/1                                         | `.HasConversion<any_numeric_type>()`
|                     | any_numeric_type       | False/true для любого из двух чисел                             | Используйте <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToTwoValuesConverter%601>.
|                     | строка                 | False/true для "Y"/"N"                                     | `.HasConversion<string>()`
|                     | строка                 | False/true для любых двух строк                             | Используйте <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToStringConverter>.
| any_numeric_type    | bool                   | 0/1 в значение false/true                                         | `.HasConversion<bool>()`
|                     | any_numeric_type       | Простое приведение                                               | `.HasConversion<any_numeric_type>()`
|                     | строка                 | Число в виде строки                                    | `.HasConversion<string>()`
| Перечисление                | any_numeric_type       | Числовое значение перечисления                             | `.HasConversion<any_numeric_type>()`
|                     | строка                 | Строковое представление значения перечисления               | `.HasConversion<string>()`
| строка              | bool                   | Анализирует строку как логическое значение                               | `.HasConversion<bool>()`
|                     | any_numeric_type       | Анализирует строку как заданный числовой тип               | `.HasConversion<any_numeric_type>()`
|                     | char                   | Первый символ строки                         | `.HasConversion<char>()`
|                     | Дата и время               | Анализирует строку как дату и время                           | `.HasConversion<DateTime>()`
|                     | DateTimeOffset         | Выполняет синтаксический анализ строки как DateTimeOffset                     | `.HasConversion<DateTimeOffset>()`
|                     | TimeSpan               | Анализирует строку как интервал времени                           | `.HasConversion<TimeSpan>()`
|                     | Guid                   | Анализирует строку как идентификатор GUID                               | `.HasConversion<Guid>()`
|                     | byte[]                 | Строка в формате UTF8 bytes                                  | `.HasConversion<byte[]>()`
| char                | строка                 | Одиночная символьная строка                                 | `.HasConversion<string>()`
| Дата и время            | long                   | Кодированная Дата и время с сохранением DateTime. Kind                | `.HasConversion<long>()`
|                     | long                   | Тактов                                                     | Используйте <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToTicksConverter>.
|                     | строка                 | Инвариантная строка даты и времени языка и региональных параметров                        | `.HasConversion<string>()`
| DateTimeOffset      | long                   | Закодированные Дата и время со смещением                             | `.HasConversion<long>()`
|                     | строка                 | Инвариантная строка даты и времени языка и региональных параметров со смещением            | `.HasConversion<string>()`
| TimeSpan            | long                   | Тактов                                                     | `.HasConversion<long>()`
|                     | строка                 | Строка инвариантного периода времени культуры                        | `.HasConversion<string>()`
| URI                 | строка                 | Универсальный код ресурса (URI) в виде строки                                       | `.HasConversion<string>()`
| PhysicalAddress     | строка                 | Адрес в виде строки                                   | `.HasConversion<string>()`
|                     | byte[]                 | Байт в порядке с обратным порядком байтов                         | `.HasConversion<byte[]>()`
| IPAddress           | строка                 | Адрес в виде строки                                   | `.HasConversion<string>()`
|                     | byte[]                 | Байт в порядке с обратным порядком байтов                         | `.HasConversion<byte[]>()`
| Guid                | строка                 | GUID в формате "dddddddd-dddd-dddd-dddd-dddddddddddd" | `.HasConversion<string>()`
|                     | byte[]                 | Байт в порядке двоичной сериализации .NET                  | `.HasConversion<byte[]>()`

Обратите внимание, что в этих преобразованиях предполагается, что формат значения подходит для преобразования. Например, преобразование строк в числа завершится ошибкой, если строковые значения не могут быть проанализированы как числа.

Полный список встроенных конвертеров:

* Преобразование свойств bool:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToStringConverter> -Bool для строк, таких как "Y" и "N"
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToTwoValuesConverter%601> -Bool для любых двух значений
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BoolToZeroOneConverter%601> -Bool — ноль и один
* Преобразование свойств массива байтов:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.BytesToStringConverter> — Массив байтов в строку в кодировке Base64
* Любое преобразование, для которого требуется только приведение типов
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.CastingConverter%602> — Преобразования, для которых требуется только приведение типов
* Преобразование свойств char:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.CharToStringConverter> -Char — строка одиночного символа
* Преобразование <xref:System.DateTimeOffset> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToBinaryConverter> - <xref:System.DateTimeOffset> в двоичное 64-разрядное значение
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToBytesConverter> - <xref:System.DateTimeOffset> в массив байтов
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeOffsetToStringConverter> - <xref:System.DateTimeOffset> в строку
* Преобразование <xref:System.DateTime> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToBinaryConverter> - <xref:System.DateTime> в 64-разрядное значение, включая DateTimeKind
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToStringConverter> - <xref:System.DateTime> в строку
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.DateTimeToTicksConverter> - <xref:System.DateTime> в такты
* Преобразование свойств перечисления:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.EnumToNumberConverter%602> -Перечислить в базовое число
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.EnumToStringConverter%601> -Enum в строку
* Преобразование <xref:System.Guid> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.GuidToBytesConverter> - <xref:System.Guid> в массив байтов
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.GuidToStringConverter> - <xref:System.Guid> в строку
* Преобразование <xref:System.Net.IPAddress> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.IPAddressToBytesConverter> - <xref:System.Net.IPAddress> в массив байтов
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.IPAddressToStringConverter> - <xref:System.Net.IPAddress> в строку
* Преобразование числовых (int, Double, десятичного и т. д.) свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.NumberToBytesConverter%601> — Любое числовое значение массива байтов
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.NumberToStringConverter%601> — Любое числовое значение в строке
* Преобразование <xref:System.Net.NetworkInformation.PhysicalAddress> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.PhysicalAddressToBytesConverter> - <xref:System.Net.NetworkInformation.PhysicalAddress> в массив байтов
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.PhysicalAddressToStringConverter> - <xref:System.Net.NetworkInformation.PhysicalAddress> в строку
* Преобразование строковых свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToBoolConverter> — Строки, такие как "Y" и "N", в bool
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToBytesConverter> — Строка — UTF8-байт
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToCharConverter> -Строка в символ
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToDateTimeConverter> -String в <xref:System.DateTime>
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToDateTimeOffsetConverter> -String в <xref:System.DateTimeOffset>
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToEnumConverter%601> -Строка для перечисления
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToGuidConverter> -String в <xref:System.Guid>
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToNumberConverter%601> -Строка в числовой тип
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToTimeSpanConverter> -String в <xref:System.TimeSpan>
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.StringToUriConverter> -String в <xref:System.Uri>
* Преобразование <xref:System.TimeSpan> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.TimeSpanToStringConverter> - <xref:System.TimeSpan> в строку
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.TimeSpanToTicksConverter> - <xref:System.TimeSpan> в такты
* Преобразование <xref:System.Uri> свойств:
  * <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.UriToStringConverter> - <xref:System.Uri> в строку

Обратите внимание, что все встроенные преобразователи не имеют состояния, поэтому один экземпляр может быть безопасно совместно использоваться несколькими свойствами.

## <a name="column-facets-and-mapping-hints"></a>Аспекты столбцов и указания по сопоставлению

Некоторые типы баз данных имеют аспекты, которые изменяют, как хранятся данные. Сюда входит следующее.

* Точность и масштаб для десятичных значений и столбцов даты и времени
* Размер и длина для двоичных и строковых столбцов
* Юникод для строковых столбцов

Эти аспекты можно настроить обычным способом для свойства, которое использует преобразователь значений, и будет применяться к преобразованному типу базы данных. Например, при преобразовании из перечисления в строки можно указать, что столбец базы данных должен быть не в Юникоде, и хранить до 20 символов:

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

Или при явном создании преобразователя:

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

`varchar(20)`При использовании EF Core миграций в SQL Server возникает столбец:

```sql
CREATE TABLE [Rider] (
    [Id] int NOT NULL IDENTITY,
    [Mount] varchar(20) NOT NULL,
    CONSTRAINT [PK_Rider] PRIMARY KEY ([Id]));
```

Однако если по умолчанию все `EquineBeast` столбцы должны быть `varchar(20)` , эти сведения могут быть переданы преобразователю значений как <xref:Microsoft.EntityFrameworkCore.Storage.ValueConversion.ConverterMappingHints> . Пример:

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

Теперь в любой момент, когда используется этот преобразователь, столбец базы данных будет содержать не Юникод с максимальной длиной 20. Однако это только указания, так как они переопределяются любыми аспектами, явно заданными в сопоставленном свойстве.

## <a name="examples"></a>Примеры

### <a name="simple-value-objects"></a>Простые объекты значений

В этом примере используется простой тип для заключения в оболочку примитивного типа. Это может быть полезно, если необходимо, чтобы тип в модели был более конкретным (и, следовательно, более строго типизированным), чем тип-примитив. В этом примере это тип `Dollars` , который заключает в оболочку десятичный примитив:

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

Его можно использовать в типе сущности:

<!--
        public class Order
        {
            public int Id { get; set; }

            public Dollars Price { get; set; }
        }
-->
[!code-csharp[SimpleValueObjectModel](../../../samples/core/Modeling/ValueConversions/SimpleValueObject.cs?name=SimpleValueObjectModel)]

И преобразуются в базовый объект `decimal` при хранении в базе данных:

<!--
                modelBuilder.Entity<Order>()
                    .Property(e => e.Price)
                    .HasConversion(
                        v => v.Amount,
                        v => new Dollars(v));
-->
[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/SimpleValueObject.cs?name=ConfigureImmutableStructProperty)]

> [!NOTE]
> Этот объект значения реализуется как [Структура только для чтения](/dotnet/csharp/language-reference/builtin-types/struct). Это означает, что EF Core может выполнять моментальный снимок и сравнивать значения без проблем. Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .

### <a name="composite-value-objects"></a>Составные объекты значений

В предыдущем примере тип объекта значения содержал только одно свойство. Чаще всего тип объекта значения состоит из нескольких свойств, совместно образующих понятие предметной области. Например, общий `Money` тип, который содержит как сумму, так и валюту:

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

Этот объект значения можно использовать в типе сущности, как и раньше:

<!--
        public class Order
        {
            public int Id { get; set; }

            public Money Price { get; set; }
        }
-->
[!code-csharp[CompositeValueObjectModel](../../../samples/core/Modeling/ValueConversions/CompositeValueObject.cs?name=CompositeValueObjectModel)]

В настоящее время преобразователи значений могут только преобразовывать значения в один столбец базы данных и из него. Это ограничение означает, что все значения свойств объекта должны быть закодированы в одно значение столбца. Обычно это обрабатывается путем сериализации объекта в базу данных и последующей десериализации. Например, с помощью <xref:System.Text.Json> :

<!--
                modelBuilder.Entity<Order>()
                    .Property(e => e.Price)
                    .HasConversion(
                        v => JsonSerializer.Serialize(v, null),
                        v => JsonSerializer.Deserialize<Money>(v, null));
-->
[!code-csharp[ConfigureCompositeValueObject](../../../samples/core/Modeling/ValueConversions/CompositeValueObject.cs?name=ConfigureCompositeValueObject)]

> [!NOTE]
> Мы планируем Разрешить сопоставление объекта с несколькими столбцами в EF Core 6,0, избавляя от необходимости использовать сериализацию здесь. Это осуществляется с помощью [#13947 проблем GitHub](https://github.com/dotnet/efcore/issues/13947).

> [!NOTE]
> Как и в предыдущем примере, этот объект значения реализуется как [Структура только для чтения](/dotnet/csharp/language-reference/builtin-types/struct). Это означает, что EF Core может выполнять моментальный снимок и сравнивать значения без проблем. Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .

### <a name="collections-of-primitives"></a>Коллекции примитивов

Сериализация также может использоваться для хранения коллекции примитивных значений. Пример:

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

<xref:System.Text.Json>Повторное использование:

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

`ICollection<string>` Представляет изменяемый ссылочный тип. Это означает, что <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> необходимо, чтобы EF Core могли правильно отслеживать и обнаруживать изменения. Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .

### <a name="collections-of-value-objects"></a>Коллекции объектов значений

Объединяя два предыдущих примера, мы можем создать коллекцию объектов значений. Например, рассмотрим `AnnualFinance` тип, который моделирует финансы в блоге в течение одного года:

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

Этот тип состоит из нескольких `Money` созданных ранее типов:

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

Затем можно добавить коллекцию `AnnualFinance` в наш тип сущности:

<!--
        public class Blog
        {
            public int Id { get; set; }
            public string Name { get; set; }
            
            public IList<AnnualFinance> Finances { get; set; }
        }
-->
[!code-csharp[ValueObjectCollectionModel](../../../samples/core/Modeling/ValueConversions/ValueObjectCollection.cs?name=ValueObjectCollectionModel)]

И снова Используйте сериализацию для хранения этого:

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
> Как и раньше, для этого преобразования требуется <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> . Дополнительные сведения см. в разделе [компараторы значений](xref:core/modeling/value-comparers) .

### <a name="value-objects-as-keys"></a>Объекты значений в качестве ключей

Иногда примитивные свойства ключа могут быть заключены в объекты значений для добавления дополнительного уровня безопасности типа при присвоении значений. Например, можно реализовать тип ключа для блогов и тип ключа для записей:

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

Затем их можно использовать в модели предметной области:

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

Обратите внимание, что `Blog.Id` невозможно случайно присвоить значение `PostKey` и `Post.Id` случайно назначить `BlogKey` . Аналогичным образом `Post.BlogId` свойству внешнего ключа должно быть присвоено значение `BlogKey` .

> [!NOTE]
> Отображение этого шаблона не означает, что мы рекомендуем его. Тщательно определите, помогает ли этот уровень абстракции помочь или препятствовать разработке. Кроме того, рассмотрите возможность использования переходов и созданных ключей вместо непосредственного обращения к ключевым значениям.

Эти ключевые свойства затем можно сопоставить с помощью преобразователей значений:

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
> В настоящее время ключевые свойства с преобразованиями не могут использовать созданные значения ключа. Проголосуйте за [#11597 проблем GitHub](https://github.com/dotnet/efcore/issues/11597) , чтобы это ограничение было удалено.

### <a name="use-ulong-for-timestamprowversion"></a>Использование ULong для timestamp/rowversion

SQL Server поддерживает автоматический [оптимистичный параллелизм](xref:core/saving/concurrency) с использованием [8-байтных двоичных `rowversion` / `timestamp` столбцов](/sql/t-sql/data-types/rowversion-transact-sql). Они всегда считываются из базы данных и записываются в нее с помощью 8-байтового массива. Однако массивы байтов являются изменяемыми ссылочными типами, что делает их довольно ненужными для работы. Преобразователи значений позволяют `rowversion` вместо этого сопоставляться со `ulong` свойством, что является гораздо более подходящим и простым в использовании, чем массив байтов. Например, рассмотрим `Blog` сущность с маркером параллелизма ulong:

<!--
        public class Blog
        {
            public int Id { get; set; }
            public string Name { get; set; }
            public ulong Version { get; set; }
        }
-->
[!code-csharp[ULongConcurrencyModel](../../../samples/core/Modeling/ValueConversions/ULongConcurrency.cs?name=ULongConcurrencyModel)]

Это можно сопоставить со столбцом SQL Server `rowversion` с помощью преобразователя значений:

<!--
                modelBuilder.Entity<Blog>()
                    .Property(e => e.Version)
                    .IsRowVersion()
                    .HasConversion<byte[]>();
-->
[!code-csharp[ConfigureULongConcurrency](../../../samples/core/Modeling/ValueConversions/ULongConcurrency.cs?name=ConfigureULongConcurrency)]

### <a name="specify-the-datetimekind-when-reading-dates"></a>Укажите DateTime. Kind при чтении дат

SQL Server отменяет <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> флаг при сохранении в <xref:System.DateTime> виде [`datetime`](/sql/t-sql/data-types/datetime-transact-sql) или [`datetime2`](/sql/t-sql/data-types/datetime2-transact-sql) . Это означает, что значения DateTime, поступившие из базы данных, всегда имеют значение <xref:System.DateTimeKind> `Unspecified` .

Преобразователи значений можно использовать двумя способами для решения этой проблемы. Во-первых, EF Core имеет преобразователь значений, который создает 8-байтовое непрозрачное значение, которое сохраняет `Kind` флаг. Пример:

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.PostedOn)
                    .HasConversion<long>();
-->
[!code-csharp[ConfigurePreserveDateTimeKind1](../../../samples/core/Modeling/ValueConversions/PreserveDateTimeKind.cs?name=ConfigurePreserveDateTimeKind1)]

Это позволяет смешивать значения DateTime с различными `Kind` флагами в базе данных.

Проблема с этим подходом заключается в том, что база данных больше не может распознать или не иметь распознаваемых `datetime` `datetime2` столбцов. Поэтому обычно всегда следует хранить время в формате UTC (или, реже, всегда местное время), а затем либо игнорировать флаг, `Kind` либо присвоить ему соответствующее значение с помощью преобразователя значений. Например, приведенный ниже преобразователь гарантирует, что `DateTime` значение, считанное из базы данных, будет иметь <xref:System.DateTimeKind> `UTC` :

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.LastUpdated)
                    .HasConversion(
                        v => v,
                        v => new DateTime(v.Ticks, DateTimeKind.Utc));
-->
[!code-csharp[ConfigurePreserveDateTimeKind2](../../../samples/core/Modeling/ValueConversions/PreserveDateTimeKind.cs?name=ConfigurePreserveDateTimeKind2)]

Если в экземплярах сущностей задаются смешанные значения в формате UTC, то перед вставкой можно использовать преобразователь для правильного преобразования. Пример:

<!--
                modelBuilder.Entity<Post>()
                    .Property(e => e.LastUpdated)
                    .HasConversion(
                        v => v.ToUniversalTime(),
                        v => new DateTime(v.Ticks, DateTimeKind.Utc));
-->
[!code-csharp[ConfigurePreserveDateTimeKind3](../../../samples/core/Modeling/ValueConversions/PreserveDateTimeKind.cs?name=ConfigurePreserveDateTimeKind3)]

> [!NOTE]
> Тщательно рассмотрите возможность объединить весь код доступа к базе данных, чтобы использовать время в формате UTC, только используя местное время при представлении данных для пользователей.

### <a name="use-case-insensitive-string-keys"></a>Использовать строковые ключи без учета регистра

Некоторые базы данных, в том числе SQL Server, выполняют сравнения строк без учета регистра по умолчанию. .NET, с другой стороны, выполняет сравнение строк с учетом регистра по умолчанию. Это означает, что значение внешнего ключа, например DotNet, будет соответствовать значению "DotNet" первичного ключа на SQL Server, но не будет соответствовать этому значению в EF Core. Функцию сравнения значений для ключей можно использовать для принудительного EF Core в сравнении строк без учета регистра, например в базе данных. Например, рассмотрим модель записи блога/posts со строковыми ключами:

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

Это не будет работать должным образом, если некоторые из `Post.BlogId` значений имеют разный регистр. Ошибки, вызванные этим действием, зависят от того, что делает приложение, но обычно содержат графы объектов, которые не были правильно [исправлены](xref:core/change-tracking/relationship-changes) , и (или) обновления, которые завершаются неудачей из-за неправильного значения FK. Чтобы исправить это, можно использовать компаратор значений:

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
> Сравнения строк .NET и сравнения строк базы данных могут различаться не только с учетом регистра. Этот шаблон работает для простых ключей ASCII, но может завершиться ошибкой для ключей с любым видом символов, относящихся к региональным параметрам. Дополнительные сведения см. в разделе [Параметры сортировки и чувствительность к регистру](xref:core/miscellaneous/collations-and-case-sensitivity) .

### <a name="handle-fixed-length-database-strings"></a>Обработайте строки базы данных фиксированной длины

В предыдущем примере не требуется преобразователь значений. Однако преобразователь может быть полезен для строковых типов базы данных фиксированной длины `char(20)` , таких как или `nchar(20)` . Строки фиксированной длины дополняются до полной длины при вставке значения в базу данных. Это означает, что значение ключа " `dotnet` " будет считаться обратно из базы данных как " `dotnet..............` ", где `.` представляет символ пробела. После этого они не будут правильно сравниваться с ключевыми значениями, которые не являются дополненными.

Преобразователь значений можно использовать для усечения заполнения при считывании значений ключа. Это можно сочетать с функцией сравнения значений в предыдущем примере для правильного сравнения ключей ASCII с фиксированной длиной без учета регистра. Пример:

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

### <a name="encrypt-property-values"></a>Шифровать значения свойств

Преобразователи значения можно использовать для шифрования значений свойств перед их отправкой в базу данных и последующей расшифровки. Например, при использовании в качестве замены для реального алгоритма шифрования строкового типа используется реверсирование.

<!--
                modelBuilder.Entity<User>().Property(e => e.Password).HasConversion(
                    v => new string(v.Reverse().ToArray()),
                    v => new string(v.Reverse().ToArray()));
-->
[!code-csharp[ConfigureEncryptPropertyValues](../../../samples/core/Modeling/ValueConversions/EncryptPropertyValues.cs?name=ConfigureEncryptPropertyValues)]

> [!NOTE]
> В настоящее время невозможно получить ссылку на текущий DbContext или другое состояние сеанса из преобразователя значений. Это ограничивает виды шифрования, которые можно использовать. Проголосуйте за [#11597 проблем GitHub](https://github.com/dotnet/efcore/issues/12205) , чтобы это ограничение было удалено.

> [!WARNING]
> Не забудьте понять все последствия, если вы перепланируете собственное шифрование для защиты конфиденциальных данных. Вместо этого рекомендуется использовать предварительно созданные механизмы шифрования, например [Always encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) на SQL Server.

## <a name="limitations"></a>Ограничения

Существует несколько известных текущих ограничений системы преобразования значений:

* В настоящее время нет способа указать в одном месте, что каждое свойство данного типа должно использовать один и тот же преобразователь значений. Ответьте ( 👍 ) на [вопрос GitHub #10784](https://github.com/dotnet/efcore/issues/10784) , если это необходимо.
* Как отмечалось выше, `null` невозможно преобразовать. Ответьте ( 👍 ) на [вопрос GitHub #13850](https://github.com/dotnet/efcore/issues/13850) , если это необходимо.
* В настоящее время невозможно распределить преобразование одного свойства в несколько столбцов или наоборот. Ответьте ( 👍 ) на [вопрос GitHub #13947](https://github.com/dotnet/efcore/issues/13947) , если это необходимо.
* Создание значения не поддерживается для большинства ключей, сопоставленных с помощью преобразователей значений. Ответьте ( 👍 ) на [вопрос GitHub #11597](https://github.com/dotnet/efcore/issues/11597) , если это необходимо.
* Преобразования значений не могут ссылаться на текущий экземпляр DbContext. Ответьте ( 👍 ) на [вопрос GitHub #11597](https://github.com/dotnet/efcore/issues/12205) , если это необходимо.

Удаление этих ограничений рассматривается в будущих выпусках.
