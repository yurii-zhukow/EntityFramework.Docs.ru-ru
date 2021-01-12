---
title: Простое ведение журнала — EF Core
description: Ведение журнала EF Core DbContext с помощью Логто
author: ajcvickers
ms.date: 10/03/2020
uid: core/logging-events-diagnostics/simple-logging
ms.openlocfilehash: 5c2dc41122dfa3919d1e6a26b0760883d77ee1a0
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129217"
---
# <a name="simple-logging"></a><span data-ttu-id="26c28-103">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="26c28-103">Simple Logging</span></span>

> [!NOTE]
> <span data-ttu-id="26c28-104">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="26c28-104">This feature was introduced in EF Core 5.0.</span></span>

> [!TIP]
> <span data-ttu-id="26c28-105">Вы можете [скачать пример этой статьи](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging/SimpleLogging) с сайта GitHub.</span><span class="sxs-lookup"><span data-stu-id="26c28-105">You can [download this article's sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging/SimpleLogging) from GitHub.</span></span>

<span data-ttu-id="26c28-106">Entity Framework Core (EF Core) простое ведение журнала можно использовать для простого получения журналов при разработке и отладке приложений.</span><span class="sxs-lookup"><span data-stu-id="26c28-106">Entity Framework Core (EF Core) simple logging can be used to easily obtain logs while developing and debugging applications.</span></span> <span data-ttu-id="26c28-107">Для этой формы ведения журнала требуется минимальная конфигурация и отсутствие дополнительных пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="26c28-107">This form of logging requires minimal configuration and no additional NuGet packages.</span></span>

> [!TIP]
> <span data-ttu-id="26c28-108">EF Core также интегрируется с [Microsoft. Extensions. Logging](xref:core/logging-events-diagnostics/extensions-logging), что требует дополнительной настройки, но часто более подходит для ведения журналов в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="26c28-108">EF Core also integrates with [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging), which requires more configuration, but is often more suitable for logging in production applications.</span></span>

## <a name="configuration"></a><span data-ttu-id="26c28-109">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="26c28-109">Configuration</span></span>

<span data-ttu-id="26c28-110">Доступ к журналам EF Core можно получить из любого типа приложений с помощью метода <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> при [настройке экземпляра DbContext](xref:core/dbcontext-configuration/index).</span><span class="sxs-lookup"><span data-stu-id="26c28-110">EF Core logs can be accessed from any type of application through the use of <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> when [configuring a DbContext instance](xref:core/dbcontext-configuration/index).</span></span> <span data-ttu-id="26c28-111">Такая конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="26c28-111">This configuration is commonly done in an override of <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="26c28-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-112">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

<span data-ttu-id="26c28-113">Кроме того, `LogTo` может вызываться как часть <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или при создании <xref:Microsoft.EntityFrameworkCore.DbContextOptions> экземпляра для передачи в `DbContext` конструктор.</span><span class="sxs-lookup"><span data-stu-id="26c28-113">Alternately, `LogTo` can be called as part of <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> or when creating a <xref:Microsoft.EntityFrameworkCore.DbContextOptions> instance to pass to the `DbContext` constructor.</span></span>

> [!TIP]
> <span data-ttu-id="26c28-114">Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="26c28-114">OnConfiguring is still called when AddDbContext is used or a DbContextOptions instance is passed to the DbContext constructor.</span></span> <span data-ttu-id="26c28-115">Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.</span><span class="sxs-lookup"><span data-stu-id="26c28-115">This makes it the ideal place to apply context configuration regardless of how the DbContext is constructed.</span></span>

## <a name="directing-the-logs"></a><span data-ttu-id="26c28-116">Направление журналов</span><span class="sxs-lookup"><span data-stu-id="26c28-116">Directing the logs</span></span>

### <a name="logging-to-the-console"></a><span data-ttu-id="26c28-117">Вход в консоль</span><span class="sxs-lookup"><span data-stu-id="26c28-117">Logging to the console</span></span>

<span data-ttu-id="26c28-118">`LogTo` требуется <xref:System.Action%601> делегат, принимающий строку.</span><span class="sxs-lookup"><span data-stu-id="26c28-118">`LogTo` requires an <xref:System.Action%601> delegate that accepts a string.</span></span> <span data-ttu-id="26c28-119">EF Core будет вызывать этот делегат со строкой для каждого созданного сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="26c28-119">EF Core will call this delegate with a string for each log message generated.</span></span> <span data-ttu-id="26c28-120">После этого он становится делегатом для того, чтобы сделать что-то с данным сообщением.</span><span class="sxs-lookup"><span data-stu-id="26c28-120">It is then up to the delegate to do something with the given message.</span></span>

<span data-ttu-id="26c28-121"><xref:System.Console.WriteLine%2A?displayProperty=nameWithType>Метод часто используется для этого делегата, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="26c28-121">The <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> method is often used for this delegate, as shown above.</span></span> <span data-ttu-id="26c28-122">Это приводит к записи каждого сообщения журнала в консоль.</span><span class="sxs-lookup"><span data-stu-id="26c28-122">This results in each log message being written to the console.</span></span>

### <a name="logging-to-the-debug-window"></a><span data-ttu-id="26c28-123">Ведение журнала в окне отладки</span><span class="sxs-lookup"><span data-stu-id="26c28-123">Logging to the debug window</span></span>

<span data-ttu-id="26c28-124"><xref:System.Diagnostics.Debug.WriteLine%2A?displayProperty=nameWithType> можно использовать для отправки выходных данных в окно отладки в Visual Studio или другом IDE.</span><span class="sxs-lookup"><span data-stu-id="26c28-124"><xref:System.Diagnostics.Debug.WriteLine%2A?displayProperty=nameWithType> can be used to send output to the Debug window in Visual Studio or other IDEs.</span></span> <span data-ttu-id="26c28-125">В этом случае следует использовать [лямбда-синтаксис](/dotnet/csharp/language-reference/operators/lambda-expressions) , так как `Debug` класс компилируется из сборок выпуска.</span><span class="sxs-lookup"><span data-stu-id="26c28-125">[Lambda syntax](/dotnet/csharp/language-reference/operators/lambda-expressions) must be used in this case because the `Debug` class is compiled out of release builds.</span></span> <span data-ttu-id="26c28-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-126">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(message => Debug.WriteLine(message));
-->
[!code-csharp[LogToDebug](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToDebug)]

### <a name="logging-to-a-file"></a><span data-ttu-id="26c28-127">Ведение журнала в файл</span><span class="sxs-lookup"><span data-stu-id="26c28-127">Logging to a file</span></span>

<span data-ttu-id="26c28-128">Для записи в файл необходимо создать <xref:System.IO.StreamWriter> или аналогичный файл.</span><span class="sxs-lookup"><span data-stu-id="26c28-128">Writing to a file requires creating a <xref:System.IO.StreamWriter> or similar for the file.</span></span> <span data-ttu-id="26c28-129"><xref:System.IO.StreamWriter.WriteLine%2A>Затем метод можно использовать, как в других примерах выше.</span><span class="sxs-lookup"><span data-stu-id="26c28-129">The <xref:System.IO.StreamWriter.WriteLine%2A> method can then be used as in the other examples above.</span></span> <span data-ttu-id="26c28-130">Не забудьте убедиться, что файл закрыт в чистом виде путем удаления модуля записи при удалении контекста.</span><span class="sxs-lookup"><span data-stu-id="26c28-130">Remember to ensure the file is closed cleanly by disposing the writer when the context is disposed.</span></span> <span data-ttu-id="26c28-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-131">For example:</span></span>

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
> <span data-ttu-id="26c28-132">Рекомендуется использовать [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging) для ведения журнала в файлах в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="26c28-132">Consider using [Microsoft.Extensions.Logging](/aspnet/core/fundamentals/logging) for logging to files in production applications.</span></span>

## <a name="getting-detailed-messages"></a><span data-ttu-id="26c28-133">Получение подробных сообщений</span><span class="sxs-lookup"><span data-stu-id="26c28-133">Getting detailed messages</span></span>

### <a name="sensitive-data"></a><span data-ttu-id="26c28-134">Конфиденциальные данные</span><span class="sxs-lookup"><span data-stu-id="26c28-134">Sensitive data</span></span>

<span data-ttu-id="26c28-135">По умолчанию EF Core не будет включать значения каких бы то ни было данных в сообщениях об исключениях.</span><span class="sxs-lookup"><span data-stu-id="26c28-135">By default, EF Core will not include the values of any data in exception messages.</span></span> <span data-ttu-id="26c28-136">Это связано с тем, что такие данные могут быть конфиденциальными и могут быть отображены в рабочей среде, если исключение не обработано.</span><span class="sxs-lookup"><span data-stu-id="26c28-136">This is because such data may be confidential, and could be revealed in production use if an exception is not handled.</span></span>

<span data-ttu-id="26c28-137">Однако знание значений данных, особенно для ключей, может быть очень полезным при отладке.</span><span class="sxs-lookup"><span data-stu-id="26c28-137">However, knowing data values, especially for keys, can be very helpful when debugging.</span></span> <span data-ttu-id="26c28-138">Это можно включить в EF Core путем вызова <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging> .</span><span class="sxs-lookup"><span data-stu-id="26c28-138">This can be enabled in EF Core by calling <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging>.</span></span> <span data-ttu-id="26c28-139">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-139">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableSensitiveDataLogging();
-->
[!code-csharp[SensitiveDataLogging](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SensitiveDataLogging)]

### <a name="detailed-query-exceptions"></a><span data-ttu-id="26c28-140">Подробные исключения запросов</span><span class="sxs-lookup"><span data-stu-id="26c28-140">Detailed query exceptions</span></span>

<span data-ttu-id="26c28-141">Из соображений производительности EF Core не заключает каждый вызов для считывания значения из поставщика базы данных в блок try-catch.</span><span class="sxs-lookup"><span data-stu-id="26c28-141">For performance reasons, EF Core does not wrap each call to read a value from the database provider in a try-catch block.</span></span> <span data-ttu-id="26c28-142">Однако это иногда приводит к возникновению исключений, которые трудно диагностировать, особенно если база данных возвращает значение NULL, если она не разрешена моделью.</span><span class="sxs-lookup"><span data-stu-id="26c28-142">However, this sometimes results in exceptions that are hard to diagnose, especially when the database returns a NULL when not allowed by the model.</span></span>

<span data-ttu-id="26c28-143">Включение приводит <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> к тому, что EF познакомит эти блоки try-catch и, таким образом, предоставит более подробные сведения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="26c28-143">Turning on <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> will cause EF to introduce these try-catch blocks and thereby provide more detailed errors.</span></span> <span data-ttu-id="26c28-144">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-144">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableDetailedErrors();
-->
[!code-csharp[EnableDetailedErrors](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EnableDetailedErrors)]

## <a name="filtering"></a><span data-ttu-id="26c28-145">Фильтрация</span><span class="sxs-lookup"><span data-stu-id="26c28-145">Filtering</span></span>

### <a name="log-levels"></a><span data-ttu-id="26c28-146">Уровни журнала</span><span class="sxs-lookup"><span data-stu-id="26c28-146">Log levels</span></span>

<span data-ttu-id="26c28-147">Каждое сообщение журнала EF Core присваивается уровню, определенному <xref:Microsoft.Extensions.Logging.LogLevel> перечислением.</span><span class="sxs-lookup"><span data-stu-id="26c28-147">Every EF Core log message is assigned to a level defined by the <xref:Microsoft.Extensions.Logging.LogLevel> enum.</span></span> <span data-ttu-id="26c28-148">По умолчанию EF Core простое ведение журнала включает каждое сообщение на `Debug` уровне или выше.</span><span class="sxs-lookup"><span data-stu-id="26c28-148">By default, EF Core simple logging includes every message at `Debug` level or above.</span></span> <span data-ttu-id="26c28-149">`LogTo` можно передать более высокий минимальный уровень, чтобы отфильтровать некоторые сообщения.</span><span class="sxs-lookup"><span data-stu-id="26c28-149">`LogTo` can be passed a higher minimum level to filter out some messages.</span></span> <span data-ttu-id="26c28-150">Например, передача `Information` результатов в минимальный набор журналов ограничивается доступом к базе данных и некоторыми сообщениями об установке.</span><span class="sxs-lookup"><span data-stu-id="26c28-150">For example, passing `Information` results in a minimal set of logs limited to database access and some housekeeping messages.</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[InfoOnly](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=InfoOnly)]

### <a name="specific-messages"></a><span data-ttu-id="26c28-151">Определенные сообщения</span><span class="sxs-lookup"><span data-stu-id="26c28-151">Specific messages</span></span>

<span data-ttu-id="26c28-152">Каждому сообщению журнала присваивается <xref:Microsoft.Extensions.Logging.EventId> .</span><span class="sxs-lookup"><span data-stu-id="26c28-152">Every log message is assigned an <xref:Microsoft.Extensions.Logging.EventId>.</span></span> <span data-ttu-id="26c28-153">Доступ к этим идентификаторам можно получить из <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> класса или <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> класса для связанных с ним сообщений.</span><span class="sxs-lookup"><span data-stu-id="26c28-153">These IDs can be accessed from the <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> class or the <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> class for relational-specific messages.</span></span> <span data-ttu-id="26c28-154">Поставщик базы данных также может иметь идентификаторы, зависящие от поставщика, в аналогичном классе.</span><span class="sxs-lookup"><span data-stu-id="26c28-154">A database provider may also have provider-specific IDs in a similar class.</span></span> <span data-ttu-id="26c28-155">Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26c28-155">For example, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> for the SQL Server provider.</span></span>

<span data-ttu-id="26c28-156">`LogTo` можно настроить для записи только в журнал сообщений, связанных с одним или несколькими идентификаторами событий.</span><span class="sxs-lookup"><span data-stu-id="26c28-156">`LogTo` can be configured to only log the messages associated with one or more event IDs.</span></span> <span data-ttu-id="26c28-157">Например, чтобы заносить в журнал только сообщения для контекста, который инициализируется или удаляется:</span><span class="sxs-lookup"><span data-stu-id="26c28-157">For example, to log only messages for the context being initialized or disposed:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { CoreEventId.ContextDisposed, CoreEventId.ContextInitialized });
-->
[!code-csharp[EventIds](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EventIds)]

### <a name="message-categories"></a><span data-ttu-id="26c28-158">Категории сообщений</span><span class="sxs-lookup"><span data-stu-id="26c28-158">Message categories</span></span>

<span data-ttu-id="26c28-159">Каждое сообщение журнала назначается именованной категории средства ведения журнала иерархии.</span><span class="sxs-lookup"><span data-stu-id="26c28-159">Every log message is assigned to a named hierarchical logger category.</span></span> <span data-ttu-id="26c28-160">Категории:</span><span class="sxs-lookup"><span data-stu-id="26c28-160">The categories are:</span></span>

| <span data-ttu-id="26c28-161">Категория</span><span class="sxs-lookup"><span data-stu-id="26c28-161">Category</span></span>                                             | <span data-ttu-id="26c28-162">Сообщения</span><span class="sxs-lookup"><span data-stu-id="26c28-162">Messages</span></span>
|:-----------------------------------------------------|-------------------------------------------------
| <span data-ttu-id="26c28-163">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="26c28-163">Microsoft.EntityFrameworkCore</span></span>                        | <span data-ttu-id="26c28-164">Все сообщения EF Core</span><span class="sxs-lookup"><span data-stu-id="26c28-164">All EF Core messages</span></span>
| <span data-ttu-id="26c28-165">Microsoft. EntityFrameworkCore. Database</span><span class="sxs-lookup"><span data-stu-id="26c28-165">Microsoft.EntityFrameworkCore.Database</span></span>               | <span data-ttu-id="26c28-166">Все взаимодействия с базой данных</span><span class="sxs-lookup"><span data-stu-id="26c28-166">All database interactions</span></span>
| <span data-ttu-id="26c28-167">Microsoft. EntityFrameworkCore. Database. Connection</span><span class="sxs-lookup"><span data-stu-id="26c28-167">Microsoft.EntityFrameworkCore.Database.Connection</span></span>    | <span data-ttu-id="26c28-168">Использование подключения к базе данных</span><span class="sxs-lookup"><span data-stu-id="26c28-168">Uses of a database connection</span></span>
| <span data-ttu-id="26c28-169">Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="26c28-169">Microsoft.EntityFrameworkCore.Database.Command</span></span>       | <span data-ttu-id="26c28-170">Использование команды базы данных</span><span class="sxs-lookup"><span data-stu-id="26c28-170">Uses of a database command</span></span>
| <span data-ttu-id="26c28-171">Microsoft. EntityFrameworkCore. Database. Transaction</span><span class="sxs-lookup"><span data-stu-id="26c28-171">Microsoft.EntityFrameworkCore.Database.Transaction</span></span>   | <span data-ttu-id="26c28-172">Использование транзакции базы данных</span><span class="sxs-lookup"><span data-stu-id="26c28-172">Uses of a database transaction</span></span>
| <span data-ttu-id="26c28-173">Microsoft. EntityFrameworkCore. Update</span><span class="sxs-lookup"><span data-stu-id="26c28-173">Microsoft.EntityFrameworkCore.Update</span></span>                 | <span data-ttu-id="26c28-174">Сохранение сущностей без взаимодействия с базой данных</span><span class="sxs-lookup"><span data-stu-id="26c28-174">Saving entities, excluding database interactions</span></span>
| <span data-ttu-id="26c28-175">Microsoft. EntityFrameworkCore. Model</span><span class="sxs-lookup"><span data-stu-id="26c28-175">Microsoft.EntityFrameworkCore.Model</span></span>                  | <span data-ttu-id="26c28-176">Все взаимодействия между моделями и метаданными</span><span class="sxs-lookup"><span data-stu-id="26c28-176">All model and metadata interactions</span></span>
| <span data-ttu-id="26c28-177">Microsoft. EntityFrameworkCore. Model. Validation</span><span class="sxs-lookup"><span data-stu-id="26c28-177">Microsoft.EntityFrameworkCore.Model.Validation</span></span>       | <span data-ttu-id="26c28-178">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="26c28-178">Model validation</span></span>
| <span data-ttu-id="26c28-179">Microsoft. EntityFrameworkCore. Query</span><span class="sxs-lookup"><span data-stu-id="26c28-179">Microsoft.EntityFrameworkCore.Query</span></span>                  | <span data-ttu-id="26c28-180">Запросы, за исключением взаимодействия с базой данных</span><span class="sxs-lookup"><span data-stu-id="26c28-180">Queries, excluding database interactions</span></span>
| <span data-ttu-id="26c28-181">Microsoft. EntityFrameworkCore. Infrastructure</span><span class="sxs-lookup"><span data-stu-id="26c28-181">Microsoft.EntityFrameworkCore.Infrastructure</span></span>         | <span data-ttu-id="26c28-182">Общие события, такие как создание контекста</span><span class="sxs-lookup"><span data-stu-id="26c28-182">General events, such as context creation</span></span>
| <span data-ttu-id="26c28-183">Microsoft. EntityFrameworkCore. формирование шаблонов</span><span class="sxs-lookup"><span data-stu-id="26c28-183">Microsoft.EntityFrameworkCore.Scaffolding</span></span>            | <span data-ttu-id="26c28-184">Реконструирование базы данных</span><span class="sxs-lookup"><span data-stu-id="26c28-184">Database reverse engineering</span></span>
| <span data-ttu-id="26c28-185">Microsoft. EntityFrameworkCore. migrations</span><span class="sxs-lookup"><span data-stu-id="26c28-185">Microsoft.EntityFrameworkCore.Migrations</span></span>             | <span data-ttu-id="26c28-186">Миграции</span><span class="sxs-lookup"><span data-stu-id="26c28-186">Migrations</span></span>
| <span data-ttu-id="26c28-187">Microsoft. EntityFrameworkCore. отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="26c28-187">Microsoft.EntityFrameworkCore.ChangeTracking</span></span>         | <span data-ttu-id="26c28-188">Взаимодействие отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="26c28-188">Change tracking interactions</span></span>

<span data-ttu-id="26c28-189">`LogTo` можно настроить для записи сообщений только из одной или нескольких категорий.</span><span class="sxs-lookup"><span data-stu-id="26c28-189">`LogTo` can be configured to only log the messages from one or more categories.</span></span> <span data-ttu-id="26c28-190">Например, чтобы вести журнал только взаимодействия с базой данных:</span><span class="sxs-lookup"><span data-stu-id="26c28-190">For example, to log only database interactions:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { DbLoggerCategory.Database.Name });
-->
[!code-csharp[DatabaseCategory](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseCategory)]

<span data-ttu-id="26c28-191">Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> класс предоставляет иерархический API для поиска категории и исключает необходимость в жестко кодировании строк.</span><span class="sxs-lookup"><span data-stu-id="26c28-191">Notice that the <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> class provides a hierarchical API for finding a category and avoids the need to hard-code strings.</span></span>

<span data-ttu-id="26c28-192">Поскольку категории являются иерархическими, в этом примере с использованием `Database` категории будут содержаться все сообщения для подкатегорий `Database.Connection` , `Database.Command` и `Database.Transaction` .</span><span class="sxs-lookup"><span data-stu-id="26c28-192">Since categories are hierarchical, this example using the `Database` category will include all messages for the subcategories `Database.Connection`, `Database.Command`, and `Database.Transaction`.</span></span>

### <a name="custom-filters"></a><span data-ttu-id="26c28-193">Настраиваемые фильтры</span><span class="sxs-lookup"><span data-stu-id="26c28-193">Custom filters</span></span>

<span data-ttu-id="26c28-194">`LogTo` позволяет использовать настраиваемый фильтр для случаев, когда ни один из параметров фильтрации не достаточен.</span><span class="sxs-lookup"><span data-stu-id="26c28-194">`LogTo` allows a custom filter to be used for cases where none of the filtering options above are sufficient.</span></span> <span data-ttu-id="26c28-195">Например, чтобы заносить в журнал любое сообщение на уровне `Information` или выше, а также сообщения для открытия и закрытия соединения:</span><span class="sxs-lookup"><span data-stu-id="26c28-195">For example, to log any message at level `Information` or above, as well as messages for opening and closing a connection:</span></span>

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
> <span data-ttu-id="26c28-196">Фильтрация с помощью настраиваемых фильтров или любого из других показанных здесь способов более эффективна, чем фильтрация в `LogTo` делегате.</span><span class="sxs-lookup"><span data-stu-id="26c28-196">Filtering using custom filters or using any of the other options shown here is more efficient than filtering in the `LogTo` delegate.</span></span> <span data-ttu-id="26c28-197">Это происходит потому, что если фильтр определяет, что сообщение не должно регистрироваться, сообщение журнала не создается даже.</span><span class="sxs-lookup"><span data-stu-id="26c28-197">This is because if the filter determines the message should not be logged, then the log message is not even created.</span></span>

## <a name="configuration-for-specific-messages"></a><span data-ttu-id="26c28-198">Конфигурация для конкретных сообщений</span><span class="sxs-lookup"><span data-stu-id="26c28-198">Configuration for specific messages</span></span>

<span data-ttu-id="26c28-199">EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API позволяет приложениям изменять то, что происходит при обнаружении определенного события.</span><span class="sxs-lookup"><span data-stu-id="26c28-199">The EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API allows applications to change what happens when a specific event is encountered.</span></span> <span data-ttu-id="26c28-200">Это можно использовать для следующих действий:</span><span class="sxs-lookup"><span data-stu-id="26c28-200">This can be used to:</span></span>

* <span data-ttu-id="26c28-201">Изменение уровня ведения журнала, в котором регистрируется событие</span><span class="sxs-lookup"><span data-stu-id="26c28-201">Change the log level at which the event is logged</span></span>
* <span data-ttu-id="26c28-202">Пропустить ведение журнала события</span><span class="sxs-lookup"><span data-stu-id="26c28-202">Skip logging the event altogether</span></span>
* <span data-ttu-id="26c28-203">Создавать исключение при возникновении события</span><span class="sxs-lookup"><span data-stu-id="26c28-203">Throw an exception when the event occurs</span></span>

### <a name="changing-the-log-level-for-an-event"></a><span data-ttu-id="26c28-204">Изменение уровня ведения журнала для события</span><span class="sxs-lookup"><span data-stu-id="26c28-204">Changing the log level for an event</span></span>

<span data-ttu-id="26c28-205">В предыдущем примере использовался настраиваемый фильтр для записи в журнал каждого сообщения, а `LogLevel.Information` также два события, определенные для `LogLevel.Debug` .</span><span class="sxs-lookup"><span data-stu-id="26c28-205">The previous example used a custom filter to log every message at `LogLevel.Information` as well as two events defined for `LogLevel.Debug`.</span></span> <span data-ttu-id="26c28-206">То же самое можно сделать, изменив уровень ведения журнала для двух `Debug` событий на `Information` :</span><span class="sxs-lookup"><span data-stu-id="26c28-206">The same can be achieved by changing the log level of the two `Debug` events to `Information`:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Log(
                (RelationalEventId.ConnectionOpened, LogLevel.Information),
                (RelationalEventId.ConnectionClosed, LogLevel.Information)))
            .LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[ChangeLogLevel](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=ChangeLogLevel)]

### <a name="suppress-logging-an-event"></a><span data-ttu-id="26c28-207">Отключить ведение журнала событий</span><span class="sxs-lookup"><span data-stu-id="26c28-207">Suppress logging an event</span></span>

<span data-ttu-id="26c28-208">Аналогичным образом отдельное событие можно подавлять из журнала.</span><span class="sxs-lookup"><span data-stu-id="26c28-208">In a similar way, an individual event can be suppressed from logging.</span></span> <span data-ttu-id="26c28-209">Это особенно полезно для пропуска предупреждения, которое было проверено и понято.</span><span class="sxs-lookup"><span data-stu-id="26c28-209">This is particularly useful for ignoring a warning that has been reviewed and understood.</span></span> <span data-ttu-id="26c28-210">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-210">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Ignore(CoreEventId.DetachedLazyLoadingWarning))
            .LogTo(Console.WriteLine);
-->
[!code-csharp[SuppressMessage](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SuppressMessage)]

### <a name="throw-for-an-event"></a><span data-ttu-id="26c28-211">Создать для события</span><span class="sxs-lookup"><span data-stu-id="26c28-211">Throw for an event</span></span>

<span data-ttu-id="26c28-212">Наконец, EF Core можно настроить для создания исключения для данного события.</span><span class="sxs-lookup"><span data-stu-id="26c28-212">Finally, EF Core can be configured to throw for a given event.</span></span> <span data-ttu-id="26c28-213">Это особенно полезно для изменения предупреждения в ошибку.</span><span class="sxs-lookup"><span data-stu-id="26c28-213">This is particularly useful for changing a warning into an error.</span></span> <span data-ttu-id="26c28-214">(Действительно, это была первоначальная цель `ConfigureWarnings` метода, и, следовательно, имя.) Например:</span><span class="sxs-lookup"><span data-stu-id="26c28-214">(Indeed, this was the original purpose of `ConfigureWarnings` method, hence the name.) For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Throw(RelationalEventId.MultipleCollectionIncludeWarning))
            .LogTo(Console.WriteLine);
-->
[!code-csharp[ThrowForEvent](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=ThrowForEvent)]

## <a name="message-contents-and-formatting"></a><span data-ttu-id="26c28-215">Содержимое и форматирование сообщения</span><span class="sxs-lookup"><span data-stu-id="26c28-215">Message contents and formatting</span></span>

<span data-ttu-id="26c28-216">Содержимое по умолчанию `LogTo` форматируется в нескольких строках.</span><span class="sxs-lookup"><span data-stu-id="26c28-216">The default content from `LogTo` is formatted across multiple lines.</span></span> <span data-ttu-id="26c28-217">Первая строка содержит метаданные сообщения:</span><span class="sxs-lookup"><span data-stu-id="26c28-217">The first line contains message metadata:</span></span>

* <span data-ttu-id="26c28-218"><xref:Microsoft.Extensions.Logging.LogLevel>Как префикс из четырех символов</span><span class="sxs-lookup"><span data-stu-id="26c28-218">The <xref:Microsoft.Extensions.Logging.LogLevel> as a four-character prefix</span></span>
* <span data-ttu-id="26c28-219">Локальная метка времени, отформатированная для текущего языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="26c28-219">A local timestamp, formatted for the current culture</span></span>
* <span data-ttu-id="26c28-220">Объект <xref:Microsoft.Extensions.Logging.EventId> в форме, который может быть скопирован или вставлен для получения члена из <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> или одного из других `EventId` классов, плюс значение НЕобработанного идентификатора.</span><span class="sxs-lookup"><span data-stu-id="26c28-220">The <xref:Microsoft.Extensions.Logging.EventId> in the form that can be copy/pasted to get the member from <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> or one of the other `EventId` classes, plus the raw ID value</span></span>
* <span data-ttu-id="26c28-221">Категория событий, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="26c28-221">The event category, as described above.</span></span>

<span data-ttu-id="26c28-222">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-222">For example:</span></span>

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

<span data-ttu-id="26c28-223">Это содержимое можно настроить путем передачи значений из <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> , как показано в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="26c28-223">This content can be customized by passing values from <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions>, as shown in the following sections.</span></span>

> [!TIP]
> <span data-ttu-id="26c28-224">Рассмотрите возможность использования [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging) для более контроля над форматированием журнала.</span><span class="sxs-lookup"><span data-stu-id="26c28-224">Consider using [Microsoft.Extensions.Logging](/aspnet/core/fundamentals/logging) for more control over log formatting.</span></span>

### <a name="using-utc-time"></a><span data-ttu-id="26c28-225">Использование времени в формате UTC</span><span class="sxs-lookup"><span data-stu-id="26c28-225">Using UTC time</span></span>

<span data-ttu-id="26c28-226">По умолчанию метки времени предназначены для локального использования во время отладки.</span><span class="sxs-lookup"><span data-stu-id="26c28-226">By default, timestamps are designed for local consumption while debugging.</span></span> <span data-ttu-id="26c28-227">Используйте <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions.DefaultWithUtcTime?displayProperty=nameWithType> , чтобы вместо этого использовать отметку времени UTC независимо от языка и региональных параметров, но хранить все остальное.</span><span class="sxs-lookup"><span data-stu-id="26c28-227">Use <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions.DefaultWithUtcTime?displayProperty=nameWithType> to use culture-agnostic UTC timestamps instead, but keep everything else the same.</span></span> <span data-ttu-id="26c28-228">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-228">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithUtcTime);
-->
[!code-csharp[Utc](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=Utc)]

<span data-ttu-id="26c28-229">В этом примере создается следующее форматирование журнала:</span><span class="sxs-lookup"><span data-stu-id="26c28-229">This example results in the following log formatting:</span></span>

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

### <a name="single-line-logging"></a><span data-ttu-id="26c28-230">Ведение журнала в одной строке</span><span class="sxs-lookup"><span data-stu-id="26c28-230">Single line logging</span></span>

<span data-ttu-id="26c28-231">Иногда бывает полезно получить ровно одну строку для каждого сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="26c28-231">Sometimes it is useful to get exactly one line per log message.</span></span> <span data-ttu-id="26c28-232">Это можно включить с помощью <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions.SingleLine?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="26c28-232">This can be enabled by <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions.SingleLine?displayProperty=nameWithType>.</span></span> <span data-ttu-id="26c28-233">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-233">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithLocalTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[SingleLine](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SingleLine)]

<span data-ttu-id="26c28-234">В этом примере создается следующее форматирование журнала:</span><span class="sxs-lookup"><span data-stu-id="26c28-234">This example results in the following log formatting:</span></span>

```output
info: 10/6/2020 10:52:45.723 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command) -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
dbug: 10/6/2020 10:52:45.723 RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committing transaction.
dbug: 10/6/2020 10:52:45.725 RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committed transaction.
```

### <a name="other-content-options"></a><span data-ttu-id="26c28-235">Другие параметры содержимого</span><span class="sxs-lookup"><span data-stu-id="26c28-235">Other content options</span></span>

<span data-ttu-id="26c28-236">Другие флаги в <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> можно использовать для сокращения количества метаданных, содержащихся в журнале.</span><span class="sxs-lookup"><span data-stu-id="26c28-236">Other flags in <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> can be used to trim down the amount of metadata included in the log.</span></span> <span data-ttu-id="26c28-237">Это может быть полезно в сочетании с однострочным ведением журнала.</span><span class="sxs-lookup"><span data-stu-id="26c28-237">This is can be useful in conjunction with single-line logging.</span></span> <span data-ttu-id="26c28-238">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-238">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.UtcTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[TerseLogs](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=TerseLogs)]

<span data-ttu-id="26c28-239">В этом примере создается следующее форматирование журнала:</span><span class="sxs-lookup"><span data-stu-id="26c28-239">This example results in the following log formatting:</span></span>

```output
2020-10-06T17:52:45.7320362Z -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
2020-10-06T17:52:45.7320531Z -> Committing transaction.
2020-10-06T17:52:45.7339441Z -> Committed transaction.
```

## <a name="moving-from-ef6"></a><span data-ttu-id="26c28-240">Перемещение из EF6</span><span class="sxs-lookup"><span data-stu-id="26c28-240">Moving from EF6</span></span>

<span data-ttu-id="26c28-241">EF Core простое ведение журнала отличается от <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6 двумя важными способами:</span><span class="sxs-lookup"><span data-stu-id="26c28-241">EF Core simple logging differs from <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> in EF6 in two important ways:</span></span>

* <span data-ttu-id="26c28-242">Сообщения журнала не ограничиваются только взаимодействием с базой данных</span><span class="sxs-lookup"><span data-stu-id="26c28-242">Log messages are not limited to only database interactions</span></span>
* <span data-ttu-id="26c28-243">Ведение журнала должно быть настроено во время инициализации контекста</span><span class="sxs-lookup"><span data-stu-id="26c28-243">The logging must be configured at context initialization time</span></span>

<span data-ttu-id="26c28-244">Для первого отличия описанная выше фильтрация может использоваться для ограничения количества сообщений, регистрируемых в журнале.</span><span class="sxs-lookup"><span data-stu-id="26c28-244">For the first difference, the filtering described above can be used to limit which messages are logged.</span></span>

<span data-ttu-id="26c28-245">Второе отличие — это преднамеренное изменение для повышения производительности, не создавая сообщения журнала, если они не нужны.</span><span class="sxs-lookup"><span data-stu-id="26c28-245">The second difference is an intentional change to improve performance by not generating log messages when they are not needed.</span></span> <span data-ttu-id="26c28-246">Однако по-прежнему можно получить аналогичное поведение для EF6 путем создания `Log` свойства в `DbContext` и последующего его использования только в том случае, если оно задано.</span><span class="sxs-lookup"><span data-stu-id="26c28-246">However, it is still possible to get a similar behavior to EF6 by creating a `Log` property on your `DbContext` and then using it only when it has been set.</span></span> <span data-ttu-id="26c28-247">Пример:</span><span class="sxs-lookup"><span data-stu-id="26c28-247">For example:</span></span>

<!--
    public Action<string> Log { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(s => Log?.Invoke(s));
-->
[!code-csharp[DatabaseLog](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseLog)]
