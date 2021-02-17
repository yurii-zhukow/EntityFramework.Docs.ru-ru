---
title: Сопоставления функций — поставщик базы данных Microsoft SQL Server — EF Core
description: Сопоставления функций поставщика Microsoft SQL Server базы данных
author: bricelam
ms.date: 1/26/2021
uid: core/providers/sql-server/functions
ms.openlocfilehash: c39e81fca52854e7fa7d5b2e81b80511e8184b90
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543228"
---
# <a name="function-mappings-of-the-microsoft-sql-server-provider"></a>Сопоставления функций поставщика Microsoft SQL Server

На этой странице показано, какие члены .NET преобразуются в функции SQL при использовании поставщика SQL Server.

## <a name="binary-functions"></a>Двоичные функции

.NET                         | SQL                           | Добавлено в
---------------------------- | ----------------------------- | --------
байт. Contains (значение)        | CHARINDEX ( @value , @bytes ) > 0 | EF Core 5.0
байт. First ()                | Подстрока ( @bytes , 1, 1)       | EF Core 6.0
байт. Недопустим                 | DATALENGTH ( @bytes )            | EF Core 5.0
байт. SequenceEqual (секунда)  | @bytes = @second              | EF Core 5.0
байт [i]                     | Подстрока ( @bytes , @i + 1, 1)  | EF Core 6.0
#. Функции. DATALENGTH (ARG) | DATALENGTH ( @arg )              | EF Core 5.0

## <a name="conversion-functions"></a>Функции преобразования

.NET                      | SQL                                    | Добавлено в
------------------------- | -------------------------------------- | --------
байт. ToString ()          | CONVERT (varchar (100), @bytes )
Битевалуе. ToString ()      | CONVERT (varchar (3), @byteValue )
Чарвалуе. ToString ()      | CONVERT (varchar (1), @charValue )
Convert. Тобулеан (значение)  | CONVERT (бит, @value )                   | EF Core 5.0
Convert. ToByte (значение)     | CONVERT (tinyint, @value )
Convert. ТодеЦимал (значение)  | CONVERT (десятичное число (18, 2), @value )
Convert. ToDouble (значение)   | CONVERT (float, @value )
Convert. ToInt16 (значение)    | CONVERT (smallint, @value )
Convert. ToInt32 (значение)    | CONVERT (int, @value )
Convert. ToInt64 (значение)    | CONVERT (bigint, @value )
Convert. ToString (значение)   | CONVERT (nvarchar (max), @value )
dateTime. ToString ()       | CONVERT (varchar (100), @dateTime )
dateTimeOffset. ToString () | CONVERT (varchar (100), @dateTimeOffset )
ДеЦималвалуе. ToString ()   | CONVERT (varchar (100), @decimalValue )
Даублевалуе. ToString ()    | CONVERT (varchar (100), @doubleValue )
Флоатвалуе. ToString ()     | CONVERT (varchar (100), @floatValue )
устройства. ToString ()           | CONVERT (varchar (36), @guid )
Интвалуе. ToString ()       | CONVERT (varchar (11), @intValue )
longValue. ToString ()      | CONVERT (varchar (20), @longValue )
Сбитевалуе. ToString ()     | CONVERT (varchar (4), @sbyteValue )
Шортвалуе. ToString ()     | CONVERT (varchar (6), @shortValue )
timeSpan. ToString ()       | CONVERT (varchar (100), @timeSpan )
Уинтвалуе. ToString ()      | CONVERT (varchar (10), @uintValue )
Улонгвалуе. ToString ()     | CONVERT (varchar (19), @ulongValue )
Ушортвалуе. ToString ()    | CONVERT (varchar (5), @ushortValue )

## <a name="date-and-time-functions"></a>Функции даты и времени

.NET                                                        | SQL                                                  | Добавлено в
----------------------------------------------------------- | ---------------------------------------------------- | --------
DateTime.Now                                                | GETDATE ()
Дата и время. сегодня                                              | CONVERT (Date, GETDATE ())
DateTime.UtcNow                                             | GETUTCDATE ()
dateTime. AddDays (значение)                                     | DATEADD (Day, @value , @dateTime )
dateTime. AddHours (значение)                                    | DATEADD (Hour, @value , @dateTime )
dateTime. Аддмиллисекондс (значение)                             | DATEADD (миллисекунда, @value , @dateTime )
dateTime. Аддминутес (значение)                                  | DATEADD (минута, @value , @dateTime )
dateTime. Аддмонсс (месяцы)                                  | DATEADD (месяц, @months , @dateTime )
dateTime. Аддсекондс (значение)                                  | DATEADD (Second, @value , @dateTime )
dateTime. Аддеарс (значение)                                    | DATEADD (year, @value , @dateTime )
Дата и время                                               | CONVERT (Дата, @dateTime )
dateTime. Day                                                | DATEPART (Day, @dateTime )
dateTime. DayOfYear                                          | DATEPART (DayOfYear, @dateTime )
dateTime. hour                                               | DATEPART (час, @dateTime )
dateTime. миллисекунд                                        | DATEPART (миллисекунда, @dateTime )
dateTime. Minute                                             | DATEPART (минута, @dateTime )
dateTime. month                                              | DATEPART (месяц, @dateTime )
dateTime. Second                                             | DATEPART (секунда, @dateTime )
dateTime. TimeOfDay                                          | CONVERT (время, @dateTime )                              | EF Core 2.2
dateTime. Year                                               | DATEPART (year, @dateTime )
DateTimeOffset.Now                                          | SYSDATETIMEOFFSET ()
DateTimeOffset. UtcNow                                       | SYSUTCDATETIME ()
dateTimeOffset. AddDays (дни)                                | DATEADD (Day, @days , @dateTimeOffset )
dateTimeOffset. AddHours (ч)                              | DATEADD (Hour, @hours , @dateTimeOffset )
dateTimeOffset. Аддмиллисекондс (миллисекунды)                | DATEADD (миллисекунда, @milliseconds , @dateTimeOffset )
dateTimeOffset. Аддминутес (минуты)                          | DATEADD (минута, @minutes , @dateTimeOffset )
dateTimeOffset. Аддмонсс (месяцы)                            | DATEADD (месяц, @months , @dateTimeOffset )
dateTimeOffset. Аддсекондс (секунды)                          | DATEADD (Second, @seconds , @dateTimeOffset )
dateTimeOffset. Аддеарс (годы)                              | DATEADD (year, @years , @dateTimeOffset )
dateTimeOffset. Date                                         | CONVERT (Дата, @dateTimeOffset )
dateTimeOffset. Day                                          | DATEPART (Day, @dateTimeOffset )
dateTimeOffset. DayOfYear                                    | DATEPART (DayOfYear, @dateTimeOffset )
dateTimeOffset. hour                                         | DATEPART (час, @dateTimeOffset )
dateTimeOffset. миллисекунд                                  | DATEPART (миллисекунда, @dateTimeOffset )
dateTimeOffset. Minute                                       | DATEPART (минута, @dateTimeOffset )
dateTimeOffset. month                                        | DATEPART (месяц, @dateTimeOffset )
dateTimeOffset. Second                                       | DATEPART (секунда, @dateTimeOffset )
dateTimeOffset. TimeOfDay                                    | CONVERT (время, @dateTimeOffset )                        | EF Core 2.2
dateTimeOffset. Year                                         | DATEPART (year, @dateTimeOffset )
#. Функции. Датедиффдай (начало, конец)                        | DATEDIFF (Day, @start , @end )
#. Функции. Датедиффхаур (начало, конец)                       | DATEDIFF (час, @start , @end )
#. Функции. Датедиффмикросеконд (начало, конец)                | DATEDIFF (микросекунда, @start , @end )
#. Функции. Датедиффмиллисеконд (начало, конец)                | DATEDIFF (миллисекунда, @start , @end )
#. Функции. Датедиффминуте (начало, конец)                     | DATEDIFF (минута, @start , @d2 )
#. Функции. Датедиффмонс (начало, конец)                      | DATEDIFF (месяц, @start , @end )
#. Функции. Датедиффнаносеконд (начало, конец)                 | DATEDIFF (наносекундных, @start , @end )
#. Функции. Датедиффсеконд (начало, конец)                     | DATEDIFF (секунды, @start , @end )
#. Функции. Датедиффвик (начало, конец)                       | DATEDIFF (неделя, @start , @end )                         | EF Core 5.0
#. Функции. Датедиффеар (начало, конец)                       | DATEDIFF (year, @start , @end )
#. Функции. DateFromParts (год, месяц, день)                | DATEFROMPARTS ( @year , @month , @day )                   | EF Core 5.0
#. Функции. DateTime2FromParts (год, месяц, день,...)      | DATETIME2FROMPARTS ( @year , @month , @day ,...)         | EF Core 5.0
#. Функции. DateTimeFromParts (год, месяц, день,...)       | DATETIMEFROMPARTS ( @year , @month , @day ,...)          | EF Core 5.0
#. Функции. DateTimeOffsetFromParts (год, месяц, день,...) | DATETIMEOFFSETFROMPARTS ( @year , @month , @day ,...)    | EF Core 5.0
#. Functions. ISDATE (выражение)                             | IsDate ( @expression )                                  | EF Core 3.0
#. Функции. SmallDateTimeFromParts (год, месяц, день,...)  | SMALLDATETIMEFROMPARTS ( @year , @month , @day ,...)     | EF Core 5.0
#. Функции. TimeFromParts (час, минута, секунда,...)       | TIMEFROMPARTS ( @hour , @minute , @second ,...)          | EF Core 5.0
timeSpan. hours                                              | DATEPART (час, @timeSpan )                            | EF Core 5.0
timeSpan. миллисекунды                                       | DATEPART (миллисекунда, @timeSpan )                     | EF Core 5.0
timeSpan. minutes                                            | DATEPART (минута, @timeSpan )                          | EF Core 5.0
timeSpan. Seconds                                            | DATEPART (секунда, @timeSpan )                          | EF Core 5.0

## <a name="numeric-functions"></a>Числовые функции

.NET                    | SQL                  | Добавлено в
----------------------- | -------------------- | --------
#. Functions. Random ()   | RAND ()               | EF Core 6.0
Math. ABS (значение)         | ABS ( @value )
Math. ACOS (d)            | ACOS ( @d )
Math. ASIN (d)            | ASIN ( @d )
Math. ATAN (d)            | ATAN ( @d )
Math. atan2 (y, x)        | ATN2 ( @y , @x )
Math. ceiling (d)         | CEILING ( @d )
Math. cos (d)             | COS ( @d )
Math. exp (d)             | EXP ( @d )
Math. Floor (d)           | FLOOR ( @d )
Math. log (d)             | LOG ( @d )
Math. log (а, Невбасе)    | Журнал ( @a , @newBase )
Math. log10 (d)           | LOG10 ( @d )
Math. Pow (x, y)          | МОЩНОСТЬ ( @x , @y )
Math. Round (d)           | ROUND ( @d , 0)
Math. Round (d, десятичные знаки) | ROUND ( @d , @decimals )
Math. Sign (значение)        | SIGN ( @value )
Math. Sin (a)             | SIN ( @a )
Math. Sqrt (d)            | SQRT ( @d )
Math. Tan (a)             | TAN ( @a )
Math. truncate (d)        | ROUND ( @d , 0, 1)

## <a name="string-functions"></a>Строковые функции

.NET                                                                    | SQL                                                                    | Добавлено в
----------------------------------------------------------------------- | ---------------------------------------------------------------------- | --------
#. Functions. COLLATE (операнд, параметры сортировки)                                | @operand COLLATE @collation                                            | EF Core 5.0
#. Функции. Contains (propertyReference, Сеарчкондитион)               | СОДЕРЖИТ ( @propertyReference , @searchCondition )                         | EF Core 2.2
#. Функции. Contains (propertyReference, Сеарчкондитион, Лангуажетерм) | СОДЕРЖИТ ( @propertyReference , @searchCondition , Language @languageTerm ) | EF Core 2.2
#. Functions. FreeText (propertyReference, freeText)                      | FREETEXT ( @propertyReference , @freeText )
#. Functions. FreeText (propertyReference, freeText, Лангуажетерм)        | FREETEXT ( @propertyReference , @freeText , язык @languageTerm )
#. Функции. ISNUMERIC (выражение)                                      | ISNUMERIC ( @expression )                                                 | EF Core 6.0
#. Функции. like (matchExpression, шаблон)                             | @matchExpression Например @pattern
#. Функции. like (matchExpression, pattern, Ескапечарактер)            | @matchExpression КАК в @pattern escape-последовательности @escapeCharacter
String. Compare (strA, strB)                                              | CASE if @strA = @strB then 0... КОНЦЕ
Строка. Concat (Str0, str1)                                               | @str0 + @str1
Строка. IsNullOrEmpty (значение)                                             | @value ИМЕЕТ значение NULL или @value Like N ""
Строка. IsNullOrWhiteSpace (значение)                                        | @value ИМЕЕТ значение NULL или LTRIM (RTRIM ( @value )) = N ' '
stringValue. CompareTo (strB)                                             | CASE if @stringValue = @strB then 0... КОНЦЕ
stringValue. Contains (значение)                                             | @stringValue LIKE N '% ' + @value + N '% '
stringValue. EndsWith (значение)                                             | @stringValue КАК И N '% ' + @value
stringValue. FirstOrDefault ()                                            | Подстрока ( @stringValue , 1, 1)                                          | EF Core 5.0
stringValue. IndexOf (значение)                                              | CHARINDEX ( @value , @stringValue )-1
stringValue. LastOrDefault ()                                             | SUBSTRING ( @stringValue , len ( @stringValue ), 1)                          | EF Core 5.0
stringValue. length                                                      | LEN ( @stringValue )
stringValue. Replace ( @oldValue , @newValue )                               | Replace ( @stringValue , @oldValue , @newValue )
stringValue. StartsWith (значение)                                           | @stringValue LIKE @value + N '% '
stringValue. substring (startIndex, Length)                               | Подстрока ( @stringValue , @startIndex + 1, @length )
stringValue. ToLower ()                                                   | LOWER ( @stringValue )
stringValue. ToUpper ()                                                   | UPPER ( @stringValue )
stringValue. Trim ()                                                      | LTRIM (RTRIM ( @stringValue ))
stringValue. TrimEnd ()                                                   | RTRIM ( @stringValue )
stringValue. TrimStart ()                                                 | LTRIM ( @stringValue )

## <a name="miscellaneous-functions"></a>Прочие функции

.NET                                     | SQL                                | Добавлено в
---------------------------------------- | ---------------------------------- | --------
набор. Contains (элемент)                | @item ОКНЕ @collection               | EF Core 3.0
enumValue. HasFlag (флаг)                  | @enumValue & @flag = @flag
Guid.NewGuid()                           | NEWID()
обнуляемого. GetValueOrDefault ()             | ОБЪЕДИНЕНИЕ ( @nullable , 0)
обнуляемого. GetValueOrDefault (defaultValue) | ОБЪЕДИНЕНИЕ ( @nullable , @defaultValue )

> [!NOTE]
> Некоторые SQL были упрощены для иллюстрации целей. Фактический SQL более сложен для работы с более широким диапазоном значений.

## <a name="see-also"></a>См. также

* [Сопоставления пространственных функций](xref:core/providers/sql-server/spatial#spatial-function-mappings)
