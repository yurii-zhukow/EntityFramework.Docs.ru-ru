---
title: Сопоставления функций — поставщик базы данных SQLite — EF Core
description: Сопоставления функций для EF Core поставщика базы данных SQLite
author: bricelam
ms.date: 1/26/2021
uid: core/providers/sqlite/functions
ms.openlocfilehash: 66fb24219465b8c34407d279450b7d501e7a572d
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543215"
---
# <a name="function-mappings-of-the-sqlite-ef-core-provider"></a>Сопоставления функций для поставщика EF Core SQLite

На этой странице показано, какие члены .NET преобразуются в функции SQL при использовании поставщика SQLite.

## <a name="binary-functions"></a>Двоичные функции

.NET                                           | SQL                                  | Добавлено в
---------------------------------------------- | ------------------------------------ | --------
байт. Contains (значение)                          | InStr ( @bytes , char ( @value )) > 0      | EF Core 5.0
байт. Недопустим                                   | Length ( @bytes )                       | EF Core 5.0
байт. SequenceEqual (секунда)                    | @bytes = @second                     | EF Core 5.0
#. Функции. hex (байты)                        | Hex ( @bytes )                          | EF Core 6.0
#. Functions. substr (Bytes, startIndex)         | substr ( @bytes , @startIndex )          | EF Core 6.0
#. Functions. substr (Bytes, startIndex, Length) | substr ( @bytes , @startIndex , @length ) | EF Core 6.0

## <a name="conversion-functions"></a>Функции преобразования

.NET                      | SQL                           | Добавлено в
------------------------- | ----------------------------- | --------
Булвалуе. ToString ()      | приведение ( @boolValue как текст)      | EF Core 6.0
Битевалуе. ToString ()      | приведение ( @byteValue как текст)      | EF Core 6.0
байт. ToString ()          | приведение ( @bytes как текст)          | EF Core 6.0
Чарвалуе. ToString ()      | приведение ( @charValue как текст)      | EF Core 6.0
dateTime. ToString ()       | приведение ( @dateTime как текст)       | EF Core 6.0
dateTimeOffset. ToString () | приведение ( @dateTimeOffset как текст) | EF Core 6.0
ДеЦималвалуе. ToString ()   | приведение ( @decimalValue как текст)   | EF Core 6.0
Даублевалуе. ToString ()    | приведение ( @doubleValue как текст)    | EF Core 6.0
Флоатвалуе. ToString ()     | приведение ( @floatValue как текст)     | EF Core 6.0
устройства. ToString ()           | приведение ( @guid как текст)           | EF Core 6.0
Интвалуе. ToString ()       | приведение ( @intValue как текст)       | EF Core 6.0
longValue. ToString ()      | приведение ( @longValue как текст)      | EF Core 6.0
Сбитевалуе. ToString ()     | приведение ( @sbyteValue как текст)     | EF Core 6.0
Шортвалуе. ToString ()     | приведение ( @shortValue как текст)     | EF Core 6.0
timeSpan. ToString ()       | приведение ( @timeSpan как текст)       | EF Core 6.0
Уинтвалуе. ToString ()      | приведение ( @uintValue как текст)      | EF Core 6.0
Ушортвалуе. ToString ()    | приведение ( @ushortValue как текст)    | EF Core 6.0

## <a name="date-and-time-functions"></a>Функции даты и времени

.NET                            | SQL                                                                      | Добавлено в
------------------------------- | ------------------------------------------------------------------------ | --------
DateTime.Now                    | DateTime (' Now ', ' localtime ')
Дата и время. сегодня                  | DateTime (' Now ', ' localtime ', ' начало дня ')
DateTime.UtcNow                 | DateTime (' Now ')
dateTime. AddDays (значение)         | DateTime ( @dateTime , @value \| \| ' Days ')
dateTime. AddHours (значение)        | DateTime ( @dateTime , @d \| \| ' hours ')
dateTime. Аддмиллисекондс (значение) | DateTime ( @dateTime , ( @value /1000,0) \| \| ' секунды ')                   | EF Core 2.2
dateTime. Аддминутес (значение)      | DateTime ( @dateTime , @value \| \| ' minutes ')
dateTime. Аддмонсс (месяцы)      | DateTime ( @dateTime , @months \| \| ' months ')
dateTime. Аддсекондс (значение)      | DateTime ( @dateTime , @value \| \| ' Seconds ')
dateTime. Аддтиккс (значение)        | DateTime ( @dateTime , ( @value /10000000,0) \| \| ' секунды ')               | EF Core 2.2
dateTime. Аддеарс (значение)        | DateTime ( @dateTime , @value \| \| ' years ')
Дата и время                   | DateTime ( @dateTime , ' начало дня ')
dateTime. Day                    | strftime ("% d", @dateTime )
dateTime. DayOfWeek              | strftime ('% w ', @dateTime )                                                | EF Core 2.2
dateTime. DayOfYear              | strftime ("% j", @dateTime )
dateTime. hour                   | strftime ('% H ', @dateTime )
dateTime. миллисекунд            | (strftime ('% f ', @dateTime ) * 1000) %1000
dateTime. Minute                 | strftime ("% M", @dateTime )
dateTime. month                  | strftime ("% m", @dateTime )
dateTime. Second                 | strftime ("% S", @dateTime )
dateTime. Ticks                  | (жулиандай ( @dateTime )-жулиандай (' 0001-01-01 00:00:00 ')) * 864000000000 | EF Core 2.2
dateTime. TimeOfDay              | время ( @dateTime )                                                          | EF Core 3.0
dateTime. Year                   | strftime ("% Y", @dateTime )

> [!NOTE]
> Некоторые SQL были упрощены для иллюстрации целей. Фактический SQL более сложен для работы с более широким диапазоном значений.

## <a name="numeric-functions"></a>Числовые функции

.NET                  | SQL                                   | Добавлено в
--------------------- | ------------------------------------- | --------
-ДеЦималвалуе         | ef_negate ( @decimalValue )              | EF Core 5.0
ДеЦималвалуе-d      | ef_add ( @decimalValue , ef_negate ( @d ))  | EF Core 5.0
ДеЦималвалуе * г      | ef_multiply ( @decimalValue , @d )        | EF Core 5.0
ДеЦималвалуе/г      | ef_divide ( @decimalValue , @d )          | EF Core 5.0
ДеЦималвалуе% d      | ef_mod ( @decimalValue , @d )             | EF Core 5.0
ДеЦималвалуе + d      | ef_add ( @decimalValue , @d )             | EF Core 5.0
ДеЦималвалуе < d      | ef_compare ( @decimalValue , @d ) < 0     | EF Core 5.0
ДеЦималвалуе <= d     | ef_compare ( @decimalValue , @d ) <= 0    | EF Core 5.0
ДеЦималвалуе > d      | ef_compare ( @decimalValue , @d ) > 0     | EF Core 5.0
ДеЦималвалуе >= d     | ef_compare ( @decimalValue , @d ) >= 0    | EF Core 5.0
Даублевалуе% d       | ef_mod ( @doubleValue , @d )              | EF Core 5.0
#. Functions. Random () | ABS (Random ()/9223372036854780000.0) | EF Core 6.0
Флоатвалуе% d        | ef_mod ( @floatValue , @d )               | EF Core 5.0
Math. ABS (значение)       | ABS ( @value )
Math. max (val1, val2)  | Max ( @val1 , @val2 )
Math. min (val1, val2)  | min ( @val1 , @val2 )
Math. Round (d)         | Round ( @d )
Math. Round (d, цифры) | Round ( @d , @digits )

> [!TIP]
> Функции SQL с префиксом *EF* создаются EF Core.

## <a name="string-functions"></a>Строковые функции

.NET                                                         | SQL                                                    | Добавлено в
------------------------------------------------------------ | ------------------------------------------------------ | --------
типа. ToLower (c)                                              | LOWER ( @c )                                              | EF Core 6.0
типа. ToUpper (c)                                              | Upper ( @c )                                              | EF Core 6.0
#. Functions. COLLATE (операнд, параметры сортировки)                     | @operand COLLATE @collation                            | EF Core 5.0
#. Функции. стандартная маска (matchExpression, pattern)                  | стандартная маска ( @pattern , @matchExpression )                       | EF Core 6.0
#. Функции. like (matchExpression, шаблон)                  | @matchExpression Например @pattern
#. Функции. like (matchExpression, pattern, Ескапечарактер) | @matchExpression КАК в @pattern escape-последовательности @escapeCharacter
Regex. Match (вход, шаблон)                                | RegExp ( @pattern , @input )                               | EF Core 6.0
String. Compare (strA, strB)                                   | CASE if @strA = @strB then 0... КОНЦЕ
Строка. Concat (Str0, str1)                                    | @str0 \|\| @str1
Строка. IsNullOrEmpty (значение)                                  | @value ИМЕЕТ значение NULL или @value = ' '
Строка. IsNullOrWhiteSpace (значение)                             | @value ИМЕЕТ значение NULL или Trim ( @value ) = ' '
stringValue. CompareTo (strB)                                  | CASE if @stringValue = @strB then 0... КОНЦЕ
stringValue. Contains (значение)                                  | InStr ( @stringValue , @value ) > 0
stringValue. EndsWith (значение)                                  | @stringValueПОХОЖЕ на "%" \| \|@value
stringValue. FirstOrDefault ()                                 | substr ( @stringValue , 1, 1)                             | EF Core 5.0
stringValue. IndexOf (значение)                                   | InStr ( @stringValue , @value )-1
stringValue. LastOrDefault ()                                  | substr ( @stringValue , Length ( @stringValue ), 1)          | EF Core 5.0
stringValue. length                                           | Length ( @stringValue )
stringValue. Replace (oldValue, newValue)                      | Replace ( @stringValue , @oldValue , @newValue )
stringValue. StartsWith (значение)                                | @stringValueПОХОЖЕ на @value \| \| "%"
stringValue. substring (startIndex, Length)                    | substr ( @stringValue , @startIndex + 1, @length )
stringValue. ToLower ()                                        | LOWER ( @stringValue )
stringValue. ToUpper ()                                        | Upper ( @stringValue )
stringValue. Trim ()                                           | Trim ( @stringValue )
stringValue. Trim (Тримчар)                                   | Trim ( @stringValue , @trimChar )
stringValue. TrimEnd ()                                        | RTRIM ( @stringValue )
stringValue. TrimEnd (Тримчар)                                | RTRIM ( @stringValue , @trimChar )
stringValue. TrimStart ()                                      | LTRIM ( @stringValue )
stringValue. TrimStart (Тримчар)                              | LTRIM ( @stringValue , @trimChar )

> [!NOTE]
> Некоторые SQL были упрощены для иллюстрации целей. Фактический SQL более сложен для работы с более широким диапазоном значений.

## <a name="miscellaneous-functions"></a>Прочие функции

.NET                                     | SQL                                | Добавлено в
---------------------------------------- | ---------------------------------- | --------
набор. Contains (элемент)                | @item ОКНЕ @collection               | EF Core 3.0
enumValue. HasFlag (флаг)                  | @enumValue & @flag = @flag
обнуляемого. GetValueOrDefault ()             | объединение ( @nullable , 0)
обнуляемого. GetValueOrDefault (defaultValue) | объединение ( @nullable , @defaultValue )

## <a name="see-also"></a>См. также

* [Сопоставления пространственных функций](xref:core/providers/sqlite/spatial#spatial-function-mappings)
