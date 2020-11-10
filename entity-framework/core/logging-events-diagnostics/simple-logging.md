---
title: Простое ведение журнала — EF Core
description: Ведение журнала EF Core DbContext с помощью Логто
author: ajcvickers
ms.date: 10/03/2020
uid: core/logging-events-diagnostics/simple-logging
ms.openlocfilehash: 49619cc10ea098e39e71dde347e00bbc3c39b13a
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431286"
---
# <a name="simple-logging"></a>Простое ведение журнала

> [!NOTE]
> Эта функция была добавлена в EF Core 5.0.

> [!TIP]  
> Вы можете [скачать пример этой статьи](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SimpleLogging) с сайта GitHub.

Entity Framework Core (EF Core) простое ведение журнала можно использовать для простого получения журналов при разработке и отладке приложений. Для этой формы ведения журнала требуется минимальная конфигурация и отсутствие дополнительных пакетов NuGet.

> [!TIP]
> EF Core также интегрируется с [Microsoft. Extensions. Logging](xref:core/logging-events-diagnostics/extensions-logging), что требует дополнительной настройки, но часто более подходит для ведения журналов в рабочих приложениях.

## <a name="configuration"></a>Параметр Configuration

Доступ к EF Core журналам можно получить из любого типа приложений с помощью [логто](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135) <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> при [настройке экземпляра DbContext](xref:core/dbcontext-configuration/index). Такая конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

Кроме того, `LogTo` может вызываться как часть <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или при создании <xref:Microsoft.EntityFrameworkCore.DbContextOptions> экземпляра для передачи в `DbContext` конструктор.

> [!TIP]
> Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext. Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.

## <a name="directing-the-logs"></a>Направление журналов

### <a name="logging-to-the-console"></a>Вход в консоль

`LogTo` требуется <xref:System.Action%601> делегат, принимающий строку. EF Core будет вызывать этот делегат со строкой для каждого созданного сообщения журнала. После этого он становится делегатом для того, чтобы сделать что-то с данным сообщением.

<xref:System.Console.WriteLine%2A?displayProperty=nameWithType>Метод часто используется для этого делегата, как показано выше. Это приводит к записи каждого сообщения журнала в консоль.

### <a name="logging-to-the-debug-window"></a>Ведение журнала в окне отладки

<xref:System.Diagnostics.Debug.WriteLine%2A?displayProperty=nameWithType> можно использовать для отправки выходных данных в окно отладки в Visual Studio или другом IDE. В этом случае следует использовать [лямбда-синтаксис](/dotnet/csharp/language-reference/operators/lambda-expressions) , так как `Debug` класс компилируется из сборок выпуска. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(message => Debug.WriteLine(message));
-->
[!code-csharp[LogToDebug](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToDebug)]

### <a name="logging-to-a-file"></a>Ведение журнала в файл

Для записи в файл необходимо создать <xref:System.IO.StreamWriter> или аналогичный файл. <xref:System.IO.StreamWriter.WriteLine%2A>Затем метод можно использовать, как в других примерах выше. Не забудьте убедиться, что файл закрыт в чистом виде путем удаления модуля записи при удалении контекста. Пример:

<!--
    private readonly StreamWriter _logStream = new StreamWriter("mylog.txt", append: true); 

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(_logStream.WriteLine);

    public override void Dispose()
    {
        base.Dispose();
        _logStream.Dispose();
    }
    
    public override async ValueTask DisposeAsync()
    {
        await base.DisposeAsync();
        await _logStream.DisposeAsync();
    }
-->
[!code-csharp[LogToFile](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToFile)]

> [!TIP]
> Рекомендуется использовать [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging) для ведения журнала в файлах в рабочих приложениях.

## <a name="getting-detailed-messages"></a>Получение подробных сообщений

### <a name="sensitive-data"></a>Конфиденциальные данные

По умолчанию EF Core не будет включать значения каких бы то ни было данных в сообщениях об исключениях. Это связано с тем, что такие данные могут быть конфиденциальными и могут быть отображены в рабочей среде, если исключение не обработано.

Однако знание значений данных, особенно для ключей, может быть очень полезным при отладке. Это можно включить в EF Core путем вызова <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging> . Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableSensitiveDataLogging();
-->
[!code-csharp[SensitiveDataLogging](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SensitiveDataLogging)]

### <a name="detailed-query-exceptions"></a>Подробные исключения запросов

Из соображений производительности EF Core не заключает каждый вызов для считывания значения из поставщика базы данных в блок try-catch. Однако это иногда приводит к возникновению исключений, которые трудно диагностировать, особенно если база данных возвращает значение NULL, если она не разрешена моделью.

Включение приводит <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> к тому, что EF познакомит эти блоки try-catch и, таким образом, предоставит более подробные сведения об ошибках. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableDetailedErrors();
-->
[!code-csharp[EnableDetailedErrors](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EnableDetailedErrors)]

## <a name="filtering"></a>Фильтрация

### <a name="log-levels"></a>Уровни журнала

Каждое сообщение журнала EF Core присваивается уровню, определенному <xref:Microsoft.Extensions.Logging.LogLevel> перечислением. По умолчанию EF Core простое ведение журнала включает каждое сообщение на `Debug` уровне или выше. `LogTo` можно передать более высокий минимальный уровень, чтобы отфильтровать некоторые сообщения. Например, передача `Information` результатов в минимальный набор журналов ограничивается доступом к базе данных и некоторыми сообщениями об установке.

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[InfoOnly](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=InfoOnly)]

### <a name="specific-messages"></a>Определенные сообщения

Каждому сообщению журнала присваивается <xref:Microsoft.Extensions.Logging.EventId> . Доступ к этим идентификаторам можно получить из <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> класса или <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> класса для связанных с ним сообщений. Поставщик базы данных также может иметь идентификаторы, зависящие от поставщика, в аналогичном классе. Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.

`LogTo` можно настроить для записи только в журнал сообщений, связанных с одним или несколькими идентификаторами событий. Например, чтобы заносить в журнал только сообщения для контекста, который инициализируется или удаляется:  

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { CoreEventId.ContextDisposed, CoreEventId.ContextInitialized });
-->
[!code-csharp[EventIds](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EventIds)]

### <a name="message-categories"></a>Категории сообщений

Каждое сообщение журнала назначается именованной категории средства ведения журнала иерархии. Категории:

| Категория                                             | Сообщения
|:-----------------------------------------------------|-------------------------------------------------
| Microsoft.EntityFrameworkCore                        | Все сообщения EF Core
| Microsoft. EntityFrameworkCore. Database               | Все взаимодействия с базой данных
| Microsoft. EntityFrameworkCore. Database. Connection    | Использование подключения к базе данных
| Microsoft. EntityFrameworkCore. Database. Command       | Использование команды базы данных
| Microsoft. EntityFrameworkCore. Database. Transaction   | Использование транзакции базы данных
| Microsoft. EntityFrameworkCore. Update                 | Сохранение сущностей без взаимодействия с базой данных
| Microsoft. EntityFrameworkCore. Model                  | Все взаимодействия между моделями и метаданными
| Microsoft. EntityFrameworkCore. Model. Validation       | Проверка модели
| Microsoft. EntityFrameworkCore. Query                  | Запросы, за исключением взаимодействия с базой данных
| Microsoft. EntityFrameworkCore. Infrastructure         | Общие события, такие как создание контекста
| Microsoft. EntityFrameworkCore. формирование шаблонов            | Реконструирование базы данных
| Microsoft. EntityFrameworkCore. migrations             | Миграции
| Microsoft. EntityFrameworkCore. отслеживания изменений         | Взаимодействие отслеживания изменений

`LogTo` можно настроить для записи сообщений только из одной или нескольких категорий. Например, чтобы вести журнал только взаимодействия с базой данных:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { DbLoggerCategory.Database.Name });
-->
[!code-csharp[DatabaseCategory](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseCategory)]

Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> класс предоставляет иерархический API для поиска категории и исключает необходимость в жестко кодировании строк.

Поскольку категории являются иерархическими, в этом примере с использованием `Database` категории будут содержаться все сообщения для подкатегорий `Database.Connection` , `Database.Command` и `Database.Transaction` .

### <a name="custom-filters"></a>Настраиваемые фильтры

`LogTo` позволяет использовать настраиваемый фильтр для случаев, когда ни один из параметров фильтрации не достаточен. Например, чтобы заносить в журнал любое сообщение на уровне `Information` или выше, а также сообщения для открытия и закрытия соединения:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(
                Console.WriteLine,
                (eventId, logLevel) => logLevel >= LogLevel.Information
                                       || eventId == RelationalEventId.ConnectionOpened
                                       || eventId == RelationalEventId.ConnectionClosed);
-->
[!code-csharp[CustomFilter](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=CustomFilter)]

> [!TIP]
> Фильтрация с помощью настраиваемых фильтров или любого из других показанных здесь способов более эффективна, чем фильтрация в делегате Логто. Это происходит потому, что если фильтр определяет, что сообщение не должно регистрироваться, сообщение журнала не создается даже.

## <a name="configuration-for-specific-messages"></a>Конфигурация для конкретных сообщений

EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API позволяет приложениям изменять то, что происходит при обнаружении определенного события. Это можно использовать для следующих действий:

* Изменение уровня ведения журнала, в котором регистрируется событие
* Пропустить ведение журнала события
* Создавать исключение при возникновении события

### <a name="changing-the-log-level-for-an-event"></a>Изменение уровня ведения журнала для события

В предыдущем примере использовался настраиваемый фильтр для записи в журнал каждого сообщения, а `LogLevel.Information` также два события, определенные для `LogLevel.Debug` . То же самое можно сделать, изменив уровень ведения журнала для двух `Debug` событий на `Information` :

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Log(
                (RelationalEventId.ConnectionOpened, LogLevel.Information),
                (RelationalEventId.ConnectionClosed, LogLevel.Information)))
            .LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[ChangeLogLevel](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=ChangeLogLevel)]

### <a name="suppress-logging-an-event"></a>Отключить ведение журнала событий

Аналогичным образом отдельное событие можно подавлять из журнала. Это особенно полезно для пропуска предупреждения, которое было проверено и понято. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Ignore(CoreEventId.DetachedLazyLoadingWarning))
            .LogTo(Console.WriteLine);
-->
[!code-csharp[SuppressMessage](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SuppressMessage)]

### <a name="throw-for-an-event"></a>Создать для события

Наконец, EF Core можно настроить для создания исключения для данного события. Это особенно полезно для изменения предупреждения в ошибку. (Действительно, это была первоначальная цель `ConfigureWarnings` метода, и, следовательно, имя.) Например:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Throw(RelationalEventId.MultipleCollectionIncludeWarning))
            .LogTo(Console.WriteLine);
-->
[!code-csharp[ThrowForEvent](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=ThrowForEvent)]

## <a name="message-contents-and-formatting"></a>Содержимое и форматирование сообщения

Содержимое по умолчанию `LogTo` форматируется в нескольких строках. Первая строка содержит метаданные сообщения:

* <xref:Microsoft.Extensions.Logging.LogLevel>Как префикс из четырех символов
* Локальная метка времени, отформатированная для текущего языка и региональных параметров
* Объект <xref:Microsoft.Extensions.Logging.EventId> в форме, который может быть скопирован или вставлен для получения члена из <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> или одного из других `EventId` классов, плюс значение НЕобработанного идентификатора.
* Категория событий, как описано выше.

Пример:

```output
info: 10/6/2020 10:52:45.581 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE "Blogs" (
          "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,
          "Name" INTEGER NOT NULL
      );
dbug: 10/6/2020 10:52:45.582 RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committing transaction.
dbug: 10/6/2020 10:52:45.585 RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committed transaction.
```

Это содержимое можно настроить, передав значения из [дбконтекстлогжероптионс](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15) . <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> -->, как показано в следующих разделах.

> [!TIP]
> Рассмотрите возможность использования [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging) для более контроля над форматированием журнала.

### <a name="using-utc-time"></a>Использование времени в формате UTC

По умолчанию тиместамнпс предназначены для локального использования во время отладки. Используйте `DbContextLoggerOptions.DefaultWithUtcTime` , чтобы вместо этого использовать отметку времени UTC независимо от языка и региональных параметров, но хранить все остальное. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithUtcTime);
-->
[!code-csharp[Utc](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=Utc)]

В этом примере создается следующее форматирование журнала:

```output
info: 2020-10-06T17:55:39.0333701Z RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE "Blogs" (
          "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,
          "Name" INTEGER NOT NULL
      );
dbug: 2020-10-06T17:55:39.0333892Z RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committing transaction.
dbug: 2020-10-06T17:55:39.0351684Z RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committed transaction.
```

### <a name="single-line-logging"></a>Ведение журнала в одной строке

Иногда бывает полезно получить ровно одну строку для каждого сообщения журнала. Это можно включить с помощью `DbContextLoggerOptions.SingleLine` . Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithLocalTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[SingleLine](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SingleLine)]

В этом примере создается следующее форматирование журнала:

```output
info: 10/6/2020 10:52:45.723 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command) -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
dbug: 10/6/2020 10:52:45.723 RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committing transaction.
dbug: 10/6/2020 10:52:45.725 RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committed transaction.
```

### <a name="other-content-options"></a>Другие параметры содержимого

Другие флаги в [дбконтекстлогжероптионс](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15) <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> --> можно использовать для сокращения количества метаданных, содержащихся в журнале. Это может быть полезно в сочетании с однострочным ведением журнала. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.UtcTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[TerseLogs](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=TerseLogs)]

В этом примере создается следующее форматирование журнала:

```output
2020-10-06T17:52:45.7320362Z -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
2020-10-06T17:52:45.7320531Z -> Committing transaction.
2020-10-06T17:52:45.7339441Z -> Committed transaction.
```

## <a name="moving-from-ef6"></a>Перемещение из EF6

EF Core простое ведение журнала отличается от <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6 двумя важными способами:

* Сообщения журнала не ограничиваются только взаимодействием с базой данных
* Ведение журнала должно быть настроено во время инициализации контекста

Для первого отличия описанная выше фильтрация может использоваться для ограничения количества сообщений, регистрируемых в журнале.

Второе отличие — это преднамеренное изменение для повышения производительности, не создавая сообщения журнала, если они не нужны. Однако по-прежнему можно получить аналогичное поведение для EF6 путем создания `Log` свойства в `DbContext` и последующего его использования только в том случае, если оно задано. Пример:

<!--
    public Action<string> Log { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(s => Log?.Invoke(s));
-->
[!code-csharp[DatabaseLog](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseLog)]
