---
title: Простое ведение журнала — EF Core
description: Ведение журнала из Ефкоре DbContext с помощью Логто
author: ajcvickers
ms.date: 10/03/2020
uid: core/miscellaneous/events/simple-logging
ms.openlocfilehash: 2d671dd9daad16a6431b91419736ec67733e41b3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066542"
---
# <a name="simple-logging"></a><span data-ttu-id="41604-103">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="41604-103">Simple logging</span></span>

> [!NOTE]
> <span data-ttu-id="41604-104">Эта функция была добавлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="41604-104">This feature was added in EF Core 5.0.</span></span>

> [!TIP]  
> <span data-ttu-id="41604-105">Вы можете [Просмотреть и скачать пример этой статьи](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SimpleLogging) на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="41604-105">You can [view and download this article's sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SimpleLogging) on GitHub.</span></span>

<span data-ttu-id="41604-106">Entity Framework Core (EF Core) простое ведение журнала можно использовать для простого получения журналов при разработке и отладке приложений.</span><span class="sxs-lookup"><span data-stu-id="41604-106">Entity Framework Core (EF Core) simple logging can be used to easily obtain logs while developing and debugging applications.</span></span> <span data-ttu-id="41604-107">Для этой формы ведения журнала требуется минимальная конфигурация и отсутствие дополнительных пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="41604-107">This form of logging requires minimal configuration and no additional NuGet packages.</span></span>

> [!TIP]
> <span data-ttu-id="41604-108">EF Core также интегрируется с [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging), что требует дополнительной настройки, но часто более подходит для ведения журналов в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="41604-108">EF Core also integrates with [Microsoft.Extensions.Logging](/aspnet/core/fundamentals/logging), which requires more configuration, but is often more suitable for logging in production applications.</span></span>

## <a name="configuration"></a><span data-ttu-id="41604-109">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="41604-109">Configuration</span></span>

<span data-ttu-id="41604-110">Доступ к EF Core журналам можно получить из любого типа приложений с помощью [логто](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135)</span><span class="sxs-lookup"><span data-stu-id="41604-110">EF Core logs can be accessed from any type of application through use of [LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135)</span></span> <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> <span data-ttu-id="41604-111">При [настройке экземпляра DbContext](xref:core/miscellaneous/configuring-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="41604-111">when [configuring a DbContext instance](xref:core/miscellaneous/configuring-dbcontext).</span></span> <span data-ttu-id="41604-112">Эта конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="41604-112">This configuration is commonly done in an override of <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="41604-113">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-113">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

<span data-ttu-id="41604-114">Кроме того, `LogTo` может вызываться как часть <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или при создании <xref:Microsoft.EntityFrameworkCore.DbContextOptions> экземпляра для передачи в `DbContext` конструктор.</span><span class="sxs-lookup"><span data-stu-id="41604-114">Alternately, `LogTo` can be called as part of <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> or when creating a <xref:Microsoft.EntityFrameworkCore.DbContextOptions> instance to pass to the `DbContext` constructor.</span></span>

> [!TIP]
> <span data-ttu-id="41604-115">Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="41604-115">OnConfiguring is still called when AddDbContext is used or a DbContextOptions instance is passed to the DbContext constructor.</span></span> <span data-ttu-id="41604-116">Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.</span><span class="sxs-lookup"><span data-stu-id="41604-116">This makes it the ideal place to apply context configuration regardless of how the DbContext is constructed.</span></span>

## <a name="directing-the-logs"></a><span data-ttu-id="41604-117">Направление журналов</span><span class="sxs-lookup"><span data-stu-id="41604-117">Directing the logs</span></span>

### <a name="logging-to-the-console"></a><span data-ttu-id="41604-118">Вход в консоль</span><span class="sxs-lookup"><span data-stu-id="41604-118">Logging to the console</span></span>

<span data-ttu-id="41604-119">`LogTo` требуется <xref:System.Action%601> делегат, принимающий строку.</span><span class="sxs-lookup"><span data-stu-id="41604-119">`LogTo` requires an <xref:System.Action%601> delegate that accepts a string.</span></span> <span data-ttu-id="41604-120">EF Core будет вызывать этот делегат со строкой для каждого созданного сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="41604-120">EF Core will call this delegate with a string for each log message generated.</span></span> <span data-ttu-id="41604-121">После этого он становится делегатом для того, чтобы сделать что-то с данным сообщением.</span><span class="sxs-lookup"><span data-stu-id="41604-121">It is then up to the delegate to do something with the given message.</span></span>

<span data-ttu-id="41604-122"><xref:System.Console.WriteLine%2A?displayProperty=nameWithType>Метод часто используется для этого делегата, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="41604-122">The <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> method is often used for this delegate, as shown above.</span></span> <span data-ttu-id="41604-123">Это приводит к записи каждого сообщения журнала в консоль.</span><span class="sxs-lookup"><span data-stu-id="41604-123">This results in each log message being written to the console.</span></span>

### <a name="logging-to-the-debug-window"></a><span data-ttu-id="41604-124">Ведение журнала в окне отладки</span><span class="sxs-lookup"><span data-stu-id="41604-124">Logging to the debug window</span></span>

<span data-ttu-id="41604-125"><xref:System.Diagnostics.Debug.WriteLine%2A?displayProperty=nameWithType> можно использовать для отправки выходных данных в окно отладки в Visual Studio или другом IDE.</span><span class="sxs-lookup"><span data-stu-id="41604-125"><xref:System.Diagnostics.Debug.WriteLine%2A?displayProperty=nameWithType> can be used to send output to the Debug window in Visual Studio or other IDEs.</span></span> <span data-ttu-id="41604-126">В этом случае следует использовать [лямбда-синтаксис](/dotnet/csharp/language-reference/operators/lambda-expressions) , так как `Debug` класс компилируется из сборок выпуска.</span><span class="sxs-lookup"><span data-stu-id="41604-126">[Lambda syntax](/dotnet/csharp/language-reference/operators/lambda-expressions) must be used in this case because the `Debug` class is compiled out of release builds.</span></span> <span data-ttu-id="41604-127">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-127">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(message => Debug.WriteLine(message));
-->
[!code-csharp[LogToDebug](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToDebug)]

### <a name="logging-to-a-file"></a><span data-ttu-id="41604-128">Ведение журнала в файл</span><span class="sxs-lookup"><span data-stu-id="41604-128">Logging to a file</span></span>

<span data-ttu-id="41604-129">Для записи в файл необходимо создать <xref:System.IO.StreamWriter> или аналогичный файл.</span><span class="sxs-lookup"><span data-stu-id="41604-129">Writing to a file requires creating a <xref:System.IO.StreamWriter> or similar for the file.</span></span> <span data-ttu-id="41604-130"><xref:System.IO.StreamWriter.WriteLine%2A>Затем метод можно использовать, как в других примерах выше.</span><span class="sxs-lookup"><span data-stu-id="41604-130">The <xref:System.IO.StreamWriter.WriteLine%2A> method can then be used as in the other examples above.</span></span> <span data-ttu-id="41604-131">Не забудьте убедиться, что файл закрыт в чистом виде путем удаления модуля записи при удалении контекста.</span><span class="sxs-lookup"><span data-stu-id="41604-131">Remember to ensure the file is closed cleanly by disposing the writer when the context is disposed.</span></span> <span data-ttu-id="41604-132">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-132">For example:</span></span>

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
[!code-csharp[LogToFile](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToFile)]

> [!TIP]
> <span data-ttu-id="41604-133">Рекомендуется использовать [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging) для ведения журнала в файлах в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="41604-133">Consider using [Microsoft.Extensions.Logging](/aspnet/core/fundamentals/logging) for logging to files in production applications.</span></span>

## <a name="getting-detailed-messages"></a><span data-ttu-id="41604-134">Получение подробных сообщений</span><span class="sxs-lookup"><span data-stu-id="41604-134">Getting detailed messages</span></span>

### <a name="sensitive-data"></a><span data-ttu-id="41604-135">Конфиденциальные данные</span><span class="sxs-lookup"><span data-stu-id="41604-135">Sensitive data</span></span>

<span data-ttu-id="41604-136">По умолчанию EF Core не будет включать значения каких бы то ни было данных в сообщениях об исключениях.</span><span class="sxs-lookup"><span data-stu-id="41604-136">By default, EF Core will not include the values of any data in exception messages.</span></span> <span data-ttu-id="41604-137">Это связано с тем, что такие данные могут быть конфиденциальными и могут быть отображены в рабочей среде, если исключение не обработано.</span><span class="sxs-lookup"><span data-stu-id="41604-137">This is because such data may be confidential, and could be revealed in production use if an exception is not handled.</span></span>

<span data-ttu-id="41604-138">Однако знание значений данных, особенно для ключей, может быть очень полезным при отладке.</span><span class="sxs-lookup"><span data-stu-id="41604-138">However, knowing data values, especially for keys, can be very helpful when debugging.</span></span> <span data-ttu-id="41604-139">Это можно включить в EF Core путем вызова <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging> .</span><span class="sxs-lookup"><span data-stu-id="41604-139">This can be enabled in EF Core by calling <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging>.</span></span> <span data-ttu-id="41604-140">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-140">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableSensitiveDataLogging();
-->
[!code-csharp[SensitiveDataLogging](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SensitiveDataLogging)]

### <a name="detailed-query-exceptions"></a><span data-ttu-id="41604-141">Подробные исключения запросов</span><span class="sxs-lookup"><span data-stu-id="41604-141">Detailed query exceptions</span></span>

<span data-ttu-id="41604-142">Из соображений производительности EF Core не заключает каждый вызов для считывания значения из поставщика базы данных в блок try-catch.</span><span class="sxs-lookup"><span data-stu-id="41604-142">For performance reasons, EF Core does not wrap each call to read a value from the database provider in a try-catch block.</span></span> <span data-ttu-id="41604-143">Однако это иногда приводит к возникновению исключений, которые трудно диагностировать, особенно если база данных возвращает значение NULL, если она не разрешена моделью.</span><span class="sxs-lookup"><span data-stu-id="41604-143">However, this sometimes results in exceptions that are hard to diagnose, especially when the database returns a NULL when not allowed by the model.</span></span>

<span data-ttu-id="41604-144">Включение приводит <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> к тому, что EF познакомит эти блоки try-catch и, таким образом, предоставит более подробные сведения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="41604-144">Turning on <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> will cause EF to introduce these try-catch blocks and thereby provide more detailed errors.</span></span> <span data-ttu-id="41604-145">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-145">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableDetailedErrors();
-->
[!code-csharp[EnableDetailedErrors](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EnableDetailedErrors)]

## <a name="filtering"></a><span data-ttu-id="41604-146">Фильтрация</span><span class="sxs-lookup"><span data-stu-id="41604-146">Filtering</span></span>

### <a name="log-levels"></a><span data-ttu-id="41604-147">Уровни журнала</span><span class="sxs-lookup"><span data-stu-id="41604-147">Log levels</span></span>

<span data-ttu-id="41604-148">Каждое сообщение журнала EF Core присваивается уровню, определенному <xref:Microsoft.Extensions.Logging.LogLevel> перечислением.</span><span class="sxs-lookup"><span data-stu-id="41604-148">Every EF Core log message is assigned to a level defined by the <xref:Microsoft.Extensions.Logging.LogLevel> enum.</span></span> <span data-ttu-id="41604-149">По умолчанию EF Core простое ведение журнала включает каждое сообщение на `Debug` уровне или выше.</span><span class="sxs-lookup"><span data-stu-id="41604-149">By default, EF Core simple logging includes every message at `Debug` level or above.</span></span> <span data-ttu-id="41604-150">`LogTo` можно передать более высокий минимальный уровень, чтобы отфильтровать некоторые сообщения.</span><span class="sxs-lookup"><span data-stu-id="41604-150">`LogTo` can be passed a higher minimum level to filter out some messages.</span></span> <span data-ttu-id="41604-151">Например, передача `Information` результатов в минимальный набор журналов ограничивается доступом к базе данных и некоторыми сообщениями об установке.</span><span class="sxs-lookup"><span data-stu-id="41604-151">For example, passing `Information` results in a minimal set of logs limited to database access and some housekeeping messages.</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[InfoOnly](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=InfoOnly)]

### <a name="specific-messages"></a><span data-ttu-id="41604-152">Определенные сообщения</span><span class="sxs-lookup"><span data-stu-id="41604-152">Specific messages</span></span>

<span data-ttu-id="41604-153">Каждому сообщению журнала присваивается <xref:Microsoft.Extensions.Logging.EventId> .</span><span class="sxs-lookup"><span data-stu-id="41604-153">Every log message is assigned an <xref:Microsoft.Extensions.Logging.EventId>.</span></span> <span data-ttu-id="41604-154">Доступ к этим идентификаторам можно получить из <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> класса или <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> класса для связанных с ним сообщений.</span><span class="sxs-lookup"><span data-stu-id="41604-154">These IDs can be accessed from the <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> class or the <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> class for relational-specific messages.</span></span> <span data-ttu-id="41604-155">Поставщик базы данных также может иметь идентификаторы, зависящие от поставщика, в аналогичном классе.</span><span class="sxs-lookup"><span data-stu-id="41604-155">A database provider may also have provider-specific IDs in a similar class.</span></span> <span data-ttu-id="41604-156">Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="41604-156">For example, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> for the SQL Server provider.</span></span>

<span data-ttu-id="41604-157">`LogTo` можно настроить для записи только в журнал сообщений, связанных с одним или несколькими идентификаторами событий.</span><span class="sxs-lookup"><span data-stu-id="41604-157">`LogTo` can be configured to only log the messages associated with one or more event IDs.</span></span> <span data-ttu-id="41604-158">Например, чтобы заносить в журнал только сообщения для контекста, который инициализируется или удаляется:</span><span class="sxs-lookup"><span data-stu-id="41604-158">For example, to log only messages for the context being initialized or disposed:</span></span>  

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { CoreEventId.ContextDisposed, CoreEventId.ContextInitialized });
-->
[!code-csharp[EventIds](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EventIds)]

### <a name="message-categories"></a><span data-ttu-id="41604-159">Категории сообщений</span><span class="sxs-lookup"><span data-stu-id="41604-159">Message categories</span></span>

<span data-ttu-id="41604-160">Каждое сообщение журнала назначается именованной категории средства ведения журнала иерархии.</span><span class="sxs-lookup"><span data-stu-id="41604-160">Every log message is assigned to a named hierarchical logger category.</span></span> <span data-ttu-id="41604-161">Категории:</span><span class="sxs-lookup"><span data-stu-id="41604-161">The categories are:</span></span>

| <span data-ttu-id="41604-162">Категория</span><span class="sxs-lookup"><span data-stu-id="41604-162">Category</span></span>                                             | <span data-ttu-id="41604-163">Сообщения</span><span class="sxs-lookup"><span data-stu-id="41604-163">Messages</span></span>
|:-----------------------------------------------------|-------------------------------------------------
| <span data-ttu-id="41604-164">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="41604-164">Microsoft.EntityFrameworkCore</span></span>                        | <span data-ttu-id="41604-165">Все сообщения EF Core</span><span class="sxs-lookup"><span data-stu-id="41604-165">All EF Core messages</span></span>
| <span data-ttu-id="41604-166">Microsoft. EntityFrameworkCore. Database</span><span class="sxs-lookup"><span data-stu-id="41604-166">Microsoft.EntityFrameworkCore.Database</span></span>               | <span data-ttu-id="41604-167">Все взаимодействия с базой данных</span><span class="sxs-lookup"><span data-stu-id="41604-167">All database interactions</span></span>
| <span data-ttu-id="41604-168">Microsoft. EntityFrameworkCore. Database. Connection</span><span class="sxs-lookup"><span data-stu-id="41604-168">Microsoft.EntityFrameworkCore.Database.Connection</span></span>    | <span data-ttu-id="41604-169">Использование подключения к базе данных</span><span class="sxs-lookup"><span data-stu-id="41604-169">Uses of a database connection</span></span>
| <span data-ttu-id="41604-170">Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="41604-170">Microsoft.EntityFrameworkCore.Database.Command</span></span>       | <span data-ttu-id="41604-171">Использование команды базы данных</span><span class="sxs-lookup"><span data-stu-id="41604-171">Uses of a database command</span></span>
| <span data-ttu-id="41604-172">Microsoft. EntityFrameworkCore. Database. Transaction</span><span class="sxs-lookup"><span data-stu-id="41604-172">Microsoft.EntityFrameworkCore.Database.Transaction</span></span>   | <span data-ttu-id="41604-173">Использование транзакции базы данных</span><span class="sxs-lookup"><span data-stu-id="41604-173">Uses of a database transaction</span></span>
| <span data-ttu-id="41604-174">Microsoft. EntityFrameworkCore. Update</span><span class="sxs-lookup"><span data-stu-id="41604-174">Microsoft.EntityFrameworkCore.Update</span></span>                 | <span data-ttu-id="41604-175">Сохранение сущностей без взаимодействия с базой данных</span><span class="sxs-lookup"><span data-stu-id="41604-175">Saving entities, excluding database interactions</span></span>
| <span data-ttu-id="41604-176">Microsoft. EntityFrameworkCore. Model</span><span class="sxs-lookup"><span data-stu-id="41604-176">Microsoft.EntityFrameworkCore.Model</span></span>                  | <span data-ttu-id="41604-177">Все взаимодействия между моделями и метаданными</span><span class="sxs-lookup"><span data-stu-id="41604-177">All model and metadata interactions</span></span>
| <span data-ttu-id="41604-178">Microsoft. EntityFrameworkCore. Model. Validation</span><span class="sxs-lookup"><span data-stu-id="41604-178">Microsoft.EntityFrameworkCore.Model.Validation</span></span>       | <span data-ttu-id="41604-179">Проверка модели</span><span class="sxs-lookup"><span data-stu-id="41604-179">Model validation</span></span>
| <span data-ttu-id="41604-180">Microsoft. EntityFrameworkCore. Query</span><span class="sxs-lookup"><span data-stu-id="41604-180">Microsoft.EntityFrameworkCore.Query</span></span>                  | <span data-ttu-id="41604-181">Запросы, за исключением взаимодействия с базой данных</span><span class="sxs-lookup"><span data-stu-id="41604-181">Queries, excluding database interactions</span></span>
| <span data-ttu-id="41604-182">Microsoft. EntityFrameworkCore. Infrastructure</span><span class="sxs-lookup"><span data-stu-id="41604-182">Microsoft.EntityFrameworkCore.Infrastructure</span></span>         | <span data-ttu-id="41604-183">Общие события, такие как создание контекста</span><span class="sxs-lookup"><span data-stu-id="41604-183">General events, such as context creation</span></span>
| <span data-ttu-id="41604-184">Microsoft. EntityFrameworkCore. формирование шаблонов</span><span class="sxs-lookup"><span data-stu-id="41604-184">Microsoft.EntityFrameworkCore.Scaffolding</span></span>            | <span data-ttu-id="41604-185">Реконструирование базы данных</span><span class="sxs-lookup"><span data-stu-id="41604-185">Database reverse engineering</span></span>
| <span data-ttu-id="41604-186">Microsoft. EntityFrameworkCore. migrations</span><span class="sxs-lookup"><span data-stu-id="41604-186">Microsoft.EntityFrameworkCore.Migrations</span></span>             | <span data-ttu-id="41604-187">Миграции</span><span class="sxs-lookup"><span data-stu-id="41604-187">Migrations</span></span>
| <span data-ttu-id="41604-188">Microsoft. EntityFrameworkCore. отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="41604-188">Microsoft.EntityFrameworkCore.ChangeTracking</span></span>         | <span data-ttu-id="41604-189">Взаимодействие отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="41604-189">Change tracking interactions</span></span>

<span data-ttu-id="41604-190">`LogTo` можно настроить для записи сообщений только из одной или нескольких категорий.</span><span class="sxs-lookup"><span data-stu-id="41604-190">`LogTo` can be configured to only log the messages from one or more categories.</span></span> <span data-ttu-id="41604-191">Например, чтобы вести журнал только взаимодействия с базой данных:</span><span class="sxs-lookup"><span data-stu-id="41604-191">For example, to log only database interactions:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { DbLoggerCategory.Database.Name });
-->
[!code-csharp[DatabaseCategory](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseCategory)]

<span data-ttu-id="41604-192">Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> класс предоставляет иерархический API для поиска категории и исключает необходимость в жестко кодировании строк.</span><span class="sxs-lookup"><span data-stu-id="41604-192">Notice that the <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> class provides a hierarchical API for finding a category and avoids the need to hard-code strings.</span></span>

<span data-ttu-id="41604-193">Поскольку категории являются иерархическими, в этом примере с использованием `Database` категории будут содержаться все сообщения для подкатегорий `Database.Connection` , `Database.Command` и `Database.Transaction` .</span><span class="sxs-lookup"><span data-stu-id="41604-193">Since categories are hierarchical, this example using the `Database` category will include all messages for the subcategories `Database.Connection`, `Database.Command`, and `Database.Transaction`.</span></span>

### <a name="custom-filters"></a><span data-ttu-id="41604-194">Настраиваемые фильтры</span><span class="sxs-lookup"><span data-stu-id="41604-194">Custom filters</span></span>

<span data-ttu-id="41604-195">`LogTo` позволяет использовать настраиваемый фильтр для случаев, когда ни один из параметров фильтрации не достаточен.</span><span class="sxs-lookup"><span data-stu-id="41604-195">`LogTo` allows a custom filter to be used for cases where none of the filtering options above are sufficient.</span></span> <span data-ttu-id="41604-196">Например, чтобы заносить в журнал любое сообщение на уровне `Information` или выше, а также сообщения для открытия и закрытия соединения:</span><span class="sxs-lookup"><span data-stu-id="41604-196">For example, to log any message at level `Information` or above, as well as messages for opening and closing a connection:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(
                Console.WriteLine,
                (eventId, logLevel) => logLevel >= LogLevel.Information
                                       || eventId == RelationalEventId.ConnectionOpened
                                       || eventId == RelationalEventId.ConnectionClosed);
-->
[!code-csharp[CustomFilter](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=CustomFilter)]

> [!TIP]
> <span data-ttu-id="41604-197">Фильтрация с помощью настраиваемых фильтров или любого из других показанных здесь способов более эффективна, чем фильтрация в делегате Логто.</span><span class="sxs-lookup"><span data-stu-id="41604-197">Filtering using custom filters or using any of the other options shown here is more efficient than filtering in the LogTo delegate.</span></span> <span data-ttu-id="41604-198">Это происходит потому, что если фильтр определяет, что сообщение не должно регистрироваться, сообщение журнала не создается даже.</span><span class="sxs-lookup"><span data-stu-id="41604-198">This is because if the filter determines the message should not be logged, then the log message is not even created.</span></span>

## <a name="message-contents-and-formatting"></a><span data-ttu-id="41604-199">Содержимое и форматирование сообщения</span><span class="sxs-lookup"><span data-stu-id="41604-199">Message contents and formatting</span></span>

<span data-ttu-id="41604-200">Содержимое по умолчанию `LogTo` форматируется в нескольких строках.</span><span class="sxs-lookup"><span data-stu-id="41604-200">The default content from `LogTo` is formatted across multiple lines.</span></span> <span data-ttu-id="41604-201">Первая строка содержит метаданные сообщения:</span><span class="sxs-lookup"><span data-stu-id="41604-201">The first line contains message metadata:</span></span>

* <span data-ttu-id="41604-202"><xref:Microsoft.Extensions.Logging.LogLevel>Как префикс из четырех символов</span><span class="sxs-lookup"><span data-stu-id="41604-202">The <xref:Microsoft.Extensions.Logging.LogLevel> as a four-character prefix</span></span>
* <span data-ttu-id="41604-203">Локальная метка времени, отформатированная для текущего языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="41604-203">A local timestamp, formatted for the current culture</span></span>
* <span data-ttu-id="41604-204">Объект <xref:Microsoft.Extensions.Logging.EventId> в форме, который может быть скопирован или вставлен для получения члена из <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> или одного из других `EventId` классов, плюс значение НЕобработанного идентификатора.</span><span class="sxs-lookup"><span data-stu-id="41604-204">The <xref:Microsoft.Extensions.Logging.EventId> in the form that can be copy/pasted to get the member from <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> or one of the other `EventId` classes, plus the raw ID value</span></span>
* <span data-ttu-id="41604-205">Категория событий, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="41604-205">The event category, as described above.</span></span>

<span data-ttu-id="41604-206">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-206">For example:</span></span>

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

<span data-ttu-id="41604-207">Это содержимое можно настроить, передав значения из [дбконтекстлогжероптионс](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15) .</span><span class="sxs-lookup"><span data-stu-id="41604-207">This content can be customized by passing values from [DbContextLoggerOptions](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15)</span></span> <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> --><span data-ttu-id="41604-208">, как показано в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="41604-208">, as shown in the following sections.</span></span>

> [!TIP]
> <span data-ttu-id="41604-209">Рассмотрите возможность использования [Microsoft. Extensions. Logging](/aspnet/core/fundamentals/logging) для более контроля над форматированием журнала.</span><span class="sxs-lookup"><span data-stu-id="41604-209">Consider using [Microsoft.Extensions.Logging](/aspnet/core/fundamentals/logging) for more control over log formatting.</span></span>

### <a name="using-utc-time"></a><span data-ttu-id="41604-210">Использование времени в формате UTC</span><span class="sxs-lookup"><span data-stu-id="41604-210">Using UTC time</span></span>

<span data-ttu-id="41604-211">По умолчанию тиместамнпс предназначены для локального использования во время отладки.</span><span class="sxs-lookup"><span data-stu-id="41604-211">By default, timestamnps are designed for local consumption while debugging.</span></span> <span data-ttu-id="41604-212">Используйте `DbContextLoggerOptions.DefaultWithUtcTime` , чтобы вместо этого использовать отметку времени UTC независимо от языка и региональных параметров, но хранить все остальное.</span><span class="sxs-lookup"><span data-stu-id="41604-212">Use `DbContextLoggerOptions.DefaultWithUtcTime` to use culture-agnostic UTC timestamps instead, but keep everything else the same.</span></span> <span data-ttu-id="41604-213">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-213">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithUtcTime);
-->
[!code-csharp[Utc](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=Utc)]

<span data-ttu-id="41604-214">В этом примере создается следующее форматирование журнала:</span><span class="sxs-lookup"><span data-stu-id="41604-214">This example results in the following log formatting:</span></span>

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

### <a name="single-line-logging"></a><span data-ttu-id="41604-215">Ведение журнала в одной строке</span><span class="sxs-lookup"><span data-stu-id="41604-215">Single line logging</span></span>

<span data-ttu-id="41604-216">Иногда бывает полезно получить ровно одну строку для каждого сообщения журнала.</span><span class="sxs-lookup"><span data-stu-id="41604-216">Sometimes it is useful to get exactly one line per log message.</span></span> <span data-ttu-id="41604-217">Это можно включить с помощью `DbContextLoggerOptions.SingleLine` .</span><span class="sxs-lookup"><span data-stu-id="41604-217">This can be enabled by `DbContextLoggerOptions.SingleLine`.</span></span> <span data-ttu-id="41604-218">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-218">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithLocalTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[SingleLine](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SingleLine)]

<span data-ttu-id="41604-219">В этом примере создается следующее форматирование журнала:</span><span class="sxs-lookup"><span data-stu-id="41604-219">This example results in the following log formatting:</span></span>

```output
info: 10/6/2020 10:52:45.723 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command) -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
dbug: 10/6/2020 10:52:45.723 RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committing transaction.
dbug: 10/6/2020 10:52:45.725 RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committed transaction.
```

### <a name="other-content-options"></a><span data-ttu-id="41604-220">Другие параметры содержимого</span><span class="sxs-lookup"><span data-stu-id="41604-220">Other content options</span></span>

<span data-ttu-id="41604-221">Другие флаги в [дбконтекстлогжероптионс](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15)</span><span class="sxs-lookup"><span data-stu-id="41604-221">Other flags in [DbContextLoggerOptions](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15)</span></span> <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> --> <span data-ttu-id="41604-222">можно использовать для сокращения количества метаданных, содержащихся в журнале.</span><span class="sxs-lookup"><span data-stu-id="41604-222">can be used to trim down the amount of metadata included in the log.</span></span> <span data-ttu-id="41604-223">Это может быть полезно в сочетании с однострочным ведением журнала.</span><span class="sxs-lookup"><span data-stu-id="41604-223">This is can be useful in conjunction with single-line logging.</span></span> <span data-ttu-id="41604-224">Например.</span><span class="sxs-lookup"><span data-stu-id="41604-224">For example:</span></span>

<span data-ttu-id="41604-225">В этом примере создается следующее форматирование журнала:</span><span class="sxs-lookup"><span data-stu-id="41604-225">This example results in the following log formatting:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.UtcTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[TerseLogs](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=TerseLogs)]

```output
2020-10-06T17:52:45.7320362Z -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
2020-10-06T17:52:45.7320531Z -> Committing transaction.
2020-10-06T17:52:45.7339441Z -> Committed transaction.
```

## <a name="moving-from-ef6"></a><span data-ttu-id="41604-226">Перемещение из EF6</span><span class="sxs-lookup"><span data-stu-id="41604-226">Moving from EF6</span></span>

<span data-ttu-id="41604-227">EF Core простое ведение журнала отличается от <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6 двумя важными способами:</span><span class="sxs-lookup"><span data-stu-id="41604-227">EF Core simple logging differs from <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> in EF6 in two important ways:</span></span>

* <span data-ttu-id="41604-228">Сообщения журнала не ограничиваются только взаимодействием с базой данных</span><span class="sxs-lookup"><span data-stu-id="41604-228">Log messages are not limited to only database interactions</span></span>
* <span data-ttu-id="41604-229">Ведение журнала должно быть настроено во время инициализации контекста</span><span class="sxs-lookup"><span data-stu-id="41604-229">The logging must be configured at context initialization time</span></span>

<span data-ttu-id="41604-230">Для первого отличия описанная выше фильтрация может использоваться для ограничения количества сообщений, регистрируемых в журнале.</span><span class="sxs-lookup"><span data-stu-id="41604-230">For the first difference, the filtering described above can be used to limit which messages are logged.</span></span>

<span data-ttu-id="41604-231">Второе отличие — это преднамеренное изменение для повышения производительности, не создавая сообщения журнала, если они не нужны.</span><span class="sxs-lookup"><span data-stu-id="41604-231">The second difference is an intentional change to improve performance by not generating log messages when they are not needed.</span></span> <span data-ttu-id="41604-232">Однако по-прежнему можно получить аналогичное поведение для EF6 путем создания `Log` свойства в `DbContext` и последующего его использования только в том случае, если оно задано.</span><span class="sxs-lookup"><span data-stu-id="41604-232">However, it is still possible to get a similar behavior to EF6 by creating a `Log` property on your `DbContext` and then using it only when it has been set.</span></span> <span data-ttu-id="41604-233">Пример:</span><span class="sxs-lookup"><span data-stu-id="41604-233">For example:</span></span>

<!--
    public Action<string> Log { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(s => Log?.Invoke(s));
-->
[!code-csharp[DatabaseLog](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseLog)]
