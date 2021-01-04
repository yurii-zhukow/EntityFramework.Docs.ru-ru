---
title: Диагностика производительности — EF Core
description: Диагностика Entity Framework Core производительности и определение узких мест
author: roji
ms.date: 12/1/2020
uid: core/performance/performance-diagnosis
ms.openlocfilehash: 9416acf3326056ef7a5d732c4bd456dac751167b
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657738"
---
# <a name="performance-diagnosis"></a><span data-ttu-id="69474-103">Диагностика производительности</span><span class="sxs-lookup"><span data-stu-id="69474-103">Performance Diagnosis</span></span>

<span data-ttu-id="69474-104">В этом разделе обсуждаются способы обнаружения проблем с производительностью в приложении EF, а также после определения проблемной области для выявления корневой проблемы.</span><span class="sxs-lookup"><span data-stu-id="69474-104">This section discusses ways for detecting performance issues in your EF application, and once a problematic area has been identified, how to further analyze them to identify the root problem.</span></span> <span data-ttu-id="69474-105">Важно тщательно диагностировать и исследовать любые проблемы, прежде чем переходить к любым переводам, и избежать появления корня проблемы.</span><span class="sxs-lookup"><span data-stu-id="69474-105">It's important to carefully diagnose and investigate any problems before jumping to any conclusions, and to avoid assuming where the root of the issue is.</span></span>

## <a name="identifying-slow-database-commands-via-logging"></a><span data-ttu-id="69474-106">Определение длительных команд базы данных с помощью ведения журнала</span><span class="sxs-lookup"><span data-stu-id="69474-106">Identifying slow database commands via logging</span></span>

<span data-ttu-id="69474-107">В конце дня EF готовит и выполняет команды для выполнения в базе данных. в реляционной базе данных это означает, что инструкции SQL выполняются через API базы данных ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="69474-107">At the end of the day, EF prepares and executes commands to be executed against your database; with relational database, that means executing SQL statements via the ADO.NET database API.</span></span> <span data-ttu-id="69474-108">Если определенный запрос занимает слишком много времени (например, из-за отсутствия индекса), его можно обнаружить, проверив журналы выполнения команд и проверяя, сколько времени они действительно делают.</span><span class="sxs-lookup"><span data-stu-id="69474-108">If a certain query is taking too much time (e.g. because an index is missing), this can be seen discovered by inspecting command execution logs and observing how long they actually take.</span></span>

<span data-ttu-id="69474-109">EF позволяет очень легко собирать время выполнения команд с помощью [простого ведения журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft. Extensions. Logging](xref:core/logging-events-diagnostics/extensions-logging):</span><span class="sxs-lookup"><span data-stu-id="69474-109">EF makes it very easy to capture command execution times, via either [simple logging](xref:core/logging-events-diagnostics/simple-logging) or [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging):</span></span>

### <a name="simple-logging"></a>[<span data-ttu-id="69474-110">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="69474-110">Simple logging</span></span>](#tab/simple-logging)

[!code-csharp[Main](../../../samples/core/Performance/BloggingContext.cs#SimpleLogging)]

### <a name="microsoftextensionslogging"></a>[<span data-ttu-id="69474-111">Microsoft.Extensions.Logging</span><span class="sxs-lookup"><span data-stu-id="69474-111">Microsoft.Extensions.Logging</span></span>](#tab/microsoft-extensions-logging)

[!code-csharp[Main](../../../samples/core/Performance/ExtensionsLoggingContext.cs#ExtensionsLogging)]

***

<span data-ttu-id="69474-112">Если уровень ведения журнала задан как `LogLevel.Information` , EF выдает сообщение журнала для каждого выполнения команды с заданным временем:</span><span class="sxs-lookup"><span data-stu-id="69474-112">When the logging level is set at `LogLevel.Information`, EF emits a log message for each command execution with the time taken:</span></span>

```log
info: 06/12/2020 09:12:36.117 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (4ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT [b].[Id], [b].[Name]
      FROM [Blogs] AS [b]
      WHERE [b].[Name] = N'foo'
```

<span data-ttu-id="69474-113">Приведенная выше команда заняла 4 миллисекунды.</span><span class="sxs-lookup"><span data-stu-id="69474-113">The above command took 4 milliseconds.</span></span> <span data-ttu-id="69474-114">Если определенная команда занимает больше времени, чем ожидалось, вы обнаружили возможную причину проблемы с производительностью и теперь можете сосредоточиться на ней, чтобы понять, почему она работает медленно.</span><span class="sxs-lookup"><span data-stu-id="69474-114">If a certain command takes more than expected, you've found a possible culprit for a performance issue, and can now focus on it to understand why it's running slowly.</span></span> <span data-ttu-id="69474-115">Ведение журнала команд также позволяет обнаружить случаи, в которых происходит непредвиденное время обращения к базе данных. Это будет отображаться в виде нескольких команд, где ожидается только один.</span><span class="sxs-lookup"><span data-stu-id="69474-115">Command logging can also reveal cases where unexpected database roundtrips are being made; this would show up as multiple commands where only one is expected.</span></span>

> [!WARNING]
> <span data-ttu-id="69474-116">Включение ведения журнала выполнения команд в рабочей среде, как правило, является неправильной идеей.</span><span class="sxs-lookup"><span data-stu-id="69474-116">Leaving command execution logging enabled in your production environment is usually a bad idea.</span></span> <span data-ttu-id="69474-117">Собственно ведение журнала замедляет работу приложения и может быстро создавать огромные файлы журналов, которые могут заполнять диск сервера.</span><span class="sxs-lookup"><span data-stu-id="69474-117">The logging itself slows down your application, and may quickly create huge log files which can fill up your server's disk.</span></span> <span data-ttu-id="69474-118">Рекомендуется вести журнал только в течение короткого интервала времени для сбора данных. Это позволяет тщательно отслеживать работу приложения или записывать данные журнала в подготовительной системе.</span><span class="sxs-lookup"><span data-stu-id="69474-118">It's recommended to only keep logging on for a short interval of time to gather data - while carefully monitoring your application - or to capture logging data on a pre-production system.</span></span>

## <a name="correlating-database-commands-to-linq-queries"></a><span data-ttu-id="69474-119">Сопоставление команд базы данных с запросами LINQ</span><span class="sxs-lookup"><span data-stu-id="69474-119">Correlating database commands to LINQ queries</span></span>

<span data-ttu-id="69474-120">Одна из проблем с ведением журнала выполнения команды заключается в том, что иногда трудно сопоставлять SQL-запросы и запросы LINQ: команды SQL, выполняемые EF, могут выглядеть совершенно иначе, чем запросы LINQ, из которых они были созданы.</span><span class="sxs-lookup"><span data-stu-id="69474-120">One problem with command execution logging is that it's sometimes difficult to correlate SQL queries and LINQ queries: the SQL commands executed by EF can look very different from the LINQ queries from which they were generated.</span></span> <span data-ttu-id="69474-121">Чтобы помочь в этой сложности, можно использовать функцию " [теги запросов](xref:core/querying/tags) EF", которая позволяет внедрить небольшой, идентифицирующий комментарий в SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="69474-121">To help with this difficulty, you may want to use EF's [query tags](xref:core/querying/tags) feature, which allows you to inject a small, identifying comment into the SQL query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#BasicQueryTag)]

<span data-ttu-id="69474-122">Тег отображается в журналах:</span><span class="sxs-lookup"><span data-stu-id="69474-122">The tag shows up in the logs:</span></span>

```sql
-- This is my spatial query!

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="69474-123">Часто стоит добавлять в теги основные запросы приложения таким образом, чтобы журналы выполнения команд стала более удобочитаемыми.</span><span class="sxs-lookup"><span data-stu-id="69474-123">It's often worth tagging the major queries of an application in this way, to make the command execution logs more immediately readable.</span></span>

## <a name="other-interfaces-for-capturing-performance-data"></a><span data-ttu-id="69474-124">Другие интерфейсы для захвата данных о производительности</span><span class="sxs-lookup"><span data-stu-id="69474-124">Other interfaces for capturing performance data</span></span>

<span data-ttu-id="69474-125">Существуют различные альтернативы функции ведения журнала EF для захвата времени выполнения команды, что может быть более мощным.</span><span class="sxs-lookup"><span data-stu-id="69474-125">There are various alternatives to EF's logging feature for capturing command execution times, which may be more powerful.</span></span> <span data-ttu-id="69474-126">Базы данных обычно поставляются с собственными средствами трассировки и анализа производительности, которые обычно предоставляют намного более широкие сведения, относящиеся к базе данных, за исключением простого времени выполнения. Фактическая настройка, возможности и использование значительно меняются в разных базах данных.</span><span class="sxs-lookup"><span data-stu-id="69474-126">Databases typically come with their own tracing and performance analysis tools, which usually provide much richer, database-specific information beyond simple execution times; the actual setup, capabilities and usage vary considerably across databases.</span></span>

<span data-ttu-id="69474-127">Например, [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) является мощным клиентом, который может подключаться к экземпляру SQL Server и предоставлять ценные сведения об управлении и производительности.</span><span class="sxs-lookup"><span data-stu-id="69474-127">For example, [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) is a powerful client that can connect to your SQL Server  instance and provide valuable management and performance information.</span></span> <span data-ttu-id="69474-128">Этот раздел выходит за рамки этого раздела, но есть две возможности: [Монитор активности](/sql/relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio), который обеспечивает динамическую панель мониторинга активности сервера (включая наиболее ресурсоемкие запросы) и функцию [расширенных событий (XEvent)](/sql/relational-databases/extended-events/quick-start-extended-events-in-sql-server) , которая позволяет определять произвольные сеансы отслеживания данных, которые можно адаптировать к конкретным потребностям.</span><span class="sxs-lookup"><span data-stu-id="69474-128">It's beyond the scope of this section to go into the details, but two capabilities worth mentioning are the [Activity Monitor](/sql/relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio), which provides a live dashboard of server activity (including the most expensive queries), and the [Extended Events (XEvent)](/sql/relational-databases/extended-events/quick-start-extended-events-in-sql-server) feature, which allows defining arbitrary data capture sessions which can be tailored to your exact needs.</span></span> <span data-ttu-id="69474-129">[В SQL Server документации по мониторингу](/sql/relational-databases/performance/monitor-and-tune-for-performance) содержатся дополнительные сведения об этих функциях, а также о других.</span><span class="sxs-lookup"><span data-stu-id="69474-129">[The SQL Server documentation on monitoring](/sql/relational-databases/performance/monitor-and-tune-for-performance) provides more information on these features, as well as others.</span></span>

<span data-ttu-id="69474-130">Другим подходом к сбору данных о производительности является сбор информации, автоматически создаваемой EF или драйвером базы данных через `DiagnosticSource` интерфейс, с последующим анализом данных или отображением их на панели мониторинга.</span><span class="sxs-lookup"><span data-stu-id="69474-130">Another approach for capturing performance data is to collect information automatically emitted by either EF or the database driver via the `DiagnosticSource` interface, and then analyze that data or display it on a dashboard.</span></span> <span data-ttu-id="69474-131">Если вы используете Azure, [azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-performance) предоставляет такие мощные средства мониторинга, что позволяет интегрировать производительность базы данных и время выполнения запросов при анализе того, как быстро обслуживаются веб-запросы.</span><span class="sxs-lookup"><span data-stu-id="69474-131">If you are using Azure, then [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-performance) provides such powerful monitoring out of the box, integrating database performance and query execution times in the analysis of how quickly your web requests are being served.</span></span> <span data-ttu-id="69474-132">Дополнительные сведения об этом доступны в [учебнике по производительности Application Insights](/azure/azure-monitor/learn/tutorial-performance)и на странице " [аналитика SQL Azure](/azure/azure-monitor/insights/azure-sql)".</span><span class="sxs-lookup"><span data-stu-id="69474-132">More information on this is available in the [Application Insights performance tutorial](/azure/azure-monitor/learn/tutorial-performance), and in the [Azure SQL analytics page](/azure/azure-monitor/insights/azure-sql).</span></span>

## <a name="inspecting-query-execution-plans"></a><span data-ttu-id="69474-133">Проверка планов выполнения запросов</span><span class="sxs-lookup"><span data-stu-id="69474-133">Inspecting query execution plans</span></span>

<span data-ttu-id="69474-134">После определения проблемного запроса, требующего оптимизации, следующий шаг обычно анализирует *план выполнения* запроса.</span><span class="sxs-lookup"><span data-stu-id="69474-134">Once you've pinpointed a problematic query that requires optimization, the next step is usually analyzing the query's *execution plan*.</span></span> <span data-ttu-id="69474-135">Когда базы данных получают инструкцию SQL, они обычно создают план выполнения этого плана. Это иногда требует сложного принятия решений в зависимости от того, какие индексы были определены, сколько данных существует в таблицах и т. д. (при этом сам план обычно должен кэшироваться на сервере для обеспечения оптимальной производительности).</span><span class="sxs-lookup"><span data-stu-id="69474-135">When databases receive a SQL statement, they typically produce a plan of how that plan is to be executed; this sometimes requires complicated decision-making based on which indexes have been defined, how much data exists in tables, etc. (incidentally, the plan itself should usually be cached at the server for optimal performance).</span></span> <span data-ttu-id="69474-136">Реляционные базы данных обычно предоставляют пользователям способ просмотра плана запроса, а также вычисление затрат для различных частей запроса. Это не имеет ценности для улучшения запросов.</span><span class="sxs-lookup"><span data-stu-id="69474-136">Relational databases typically provide a way for users to see the query plan, along with calculated costing for different parts of the query; this is invaluable for improving your queries.</span></span>

<span data-ttu-id="69474-137">Чтобы приступить к работе с SQL Server, см. документацию по [планам выполнения запросов](/sql/relational-databases/performance/execution-plans).</span><span class="sxs-lookup"><span data-stu-id="69474-137">To get started on SQL Server, see the documentation on [query execution plans](/sql/relational-databases/performance/execution-plans).</span></span> <span data-ttu-id="69474-138">Типичный рабочий процесс анализа предполагает использование [SQL Server Management Studio](/sql/relational-databases/performance/display-an-actual-execution-plan), вставки SQL-запроса, определенного через одно из приведенных выше средств, и [создания графического плана выполнения](/sql/relational-databases/performance/display-an-actual-execution-plan):</span><span class="sxs-lookup"><span data-stu-id="69474-138">The typical analysis workflow would be to use [SQL Server Management Studio](/sql/relational-databases/performance/display-an-actual-execution-plan), pasting the SQL of a slow query identified via one of the means above, and [producing a graphical execution plan](/sql/relational-databases/performance/display-an-actual-execution-plan):</span></span>

![Отображение SQL Server плана выполнения](_static/actualexecplan.png)

<span data-ttu-id="69474-140">Хотя планы выполнения могут показаться сложными, на первый взгляд стоит потратить немного времени на знакомство с ними.</span><span class="sxs-lookup"><span data-stu-id="69474-140">While execution plans may seem complicated at first, it's worth spending a bit of time getting familiar with them.</span></span> <span data-ttu-id="69474-141">Особенно важно отметить затраты, связанные с каждым узлом плана, а также выяснить, как индексы используются (или не) на различных узлах.</span><span class="sxs-lookup"><span data-stu-id="69474-141">It's particularly important to note the costs associated with each node of the plan, and to identify how indexes are used (or not) in the various nodes.</span></span>

<span data-ttu-id="69474-142">Хотя приведенные выше сведения относятся к SQL Server, другие базы данных обычно предоставляют те же средства с аналогичной визуализацией.</span><span class="sxs-lookup"><span data-stu-id="69474-142">While the above information is specific to SQL Server, other databases typically provide the same kind of tools with similar visualization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69474-143">Иногда базы данных создают разные планы запросов в зависимости от фактических данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="69474-143">Databases sometimes generate different query plans depending on actual data in the database.</span></span> <span data-ttu-id="69474-144">Например, если таблица содержит всего несколько строк, то база данных может не использовать индекс для этой таблицы, а выполнять полное сканирование таблицы.</span><span class="sxs-lookup"><span data-stu-id="69474-144">For example, if a table contains only a few rows, a database may choose not to use an index on that table, but to perform a full table scan instead.</span></span> <span data-ttu-id="69474-145">При анализе планов запросов в тестовой базе данных всегда убедитесь, что она содержит данные, аналогичные вашей рабочей системе.</span><span class="sxs-lookup"><span data-stu-id="69474-145">If analyzing query plans on a test database, always make sure it contains data that is similar to your production system.</span></span>

## <a name="event-counters"></a><span data-ttu-id="69474-146">Счетчики событий</span><span class="sxs-lookup"><span data-stu-id="69474-146">Event counters</span></span>

<span data-ttu-id="69474-147">Приведенные выше разделы посвящены получению сведений о командах и способах выполнения этих команд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="69474-147">The above sections focused on how to get information about your commands, and how these commands are executed in the database.</span></span> <span data-ttu-id="69474-148">Помимо этого, EF предоставляет набор *счетчиков событий* , которые предоставляют более низкую информацию о том, что происходит внутри самой EF и как приложение использует его.</span><span class="sxs-lookup"><span data-stu-id="69474-148">In addition to that, EF exposes a set of *event counters* which provide more lower-level information on what's happening inside EF itself, and how your application is using it.</span></span> <span data-ttu-id="69474-149">Эти счетчики могут оказаться очень полезными для диагностики конкретных проблем производительности и аномалий производительности, таких как [проблемы кэширования запросов](xref:core/performance/advanced-performance-topics#dynamically-constructed-queries) , которые вызывают повторную компиляцию, неосвобожденные DbContext утечки и др.</span><span class="sxs-lookup"><span data-stu-id="69474-149">These counters can be very useful for diagnosing specific performance issues and performance anomalies, such as [query caching issues](xref:core/performance/advanced-performance-topics#dynamically-constructed-queries) which cause constant recompilation, undisposed DbContext leaks, and others.</span></span>

<span data-ttu-id="69474-150">Дополнительные сведения см. в разделе [счетчики событий EF](xref:core/logging-events-diagnostics/event-counters) на выделенной странице.</span><span class="sxs-lookup"><span data-stu-id="69474-150">See the dedicated page on [EF's event counters](xref:core/logging-events-diagnostics/event-counters) for more information.</span></span>

## <a name="benchmarking-with-ef-core"></a><span data-ttu-id="69474-151">Тестирование производительности с помощью EF Core</span><span class="sxs-lookup"><span data-stu-id="69474-151">Benchmarking with EF Core</span></span>

<span data-ttu-id="69474-152">В конце дня иногда необходимо знать, является ли определенный способ написания или выполнения запроса более быстрым, чем другой.</span><span class="sxs-lookup"><span data-stu-id="69474-152">At the end of the day, you sometimes need to know whether a particular way of writing or executing a query is faster than another.</span></span> <span data-ttu-id="69474-153">Важно никогда не предположить и не представить ответ, и очень просто создать быстрый тест производительности, чтобы получить ответ.</span><span class="sxs-lookup"><span data-stu-id="69474-153">It's important to never assume or speculate the answer, and it's extremely easy to put together a quick benchmark to get the answer.</span></span> <span data-ttu-id="69474-154">При написании тестов производительности настоятельно рекомендуется использовать хорошо известную библиотеку [бенчмаркдотнет](https://benchmarkdotnet.org/index.html) , которая обрабатывает множество пользователей, которые сталкиваются с проблемами при написании собственных тестов производительности: вы выполнили несколько итераций для прогрева?</span><span class="sxs-lookup"><span data-stu-id="69474-154">When writing benchmarks, it's strongly recommended to use the well-known [BenchmarkDotNet](https://benchmarkdotnet.org/index.html) library, which handles many pitfalls users encounter when trying to write their own benchmarks: have you performed some warmup iterations?</span></span> <span data-ttu-id="69474-155">Сколько итераций на самом деле выполняет ваш тест производительности и почему?</span><span class="sxs-lookup"><span data-stu-id="69474-155">How many iterations does your benchmark actually run, and why?</span></span> <span data-ttu-id="69474-156">Давайте рассмотрим, как выглядит тест производительности с EF Core.</span><span class="sxs-lookup"><span data-stu-id="69474-156">Let's take a look at what a benchmark with EF Core looks like.</span></span>

> [!TIP]
> <span data-ttu-id="69474-157">Полный проект теста производительности для источника ниже доступен [здесь](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Benchmarks/AverageBlogRanking.cs).</span><span class="sxs-lookup"><span data-stu-id="69474-157">The full benchmark project for the source below is available [here](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Benchmarks/AverageBlogRanking.cs).</span></span> <span data-ttu-id="69474-158">Рекомендуется скопировать его и использовать в качестве шаблона для собственных тестов производительности.</span><span class="sxs-lookup"><span data-stu-id="69474-158">You are encouraged to copy it and use it as a template for your own benchmarks.</span></span>

<span data-ttu-id="69474-159">В качестве простого сценария тестирования можно сравнить следующие методы расчета среднего рейтинга всех блогов в нашей базе данных:</span><span class="sxs-lookup"><span data-stu-id="69474-159">As a simple benchmark scenario, let's compare the following different methods of calculating the average ranking of all Blogs in our database:</span></span>

* <span data-ttu-id="69474-160">Загрузите все сущности, суммируйте их отдельные ранжирования и Вычислите среднее значение.</span><span class="sxs-lookup"><span data-stu-id="69474-160">Load all entities, sum up their individual rankings, and calculate the average.</span></span>
* <span data-ttu-id="69474-161">То же, что и выше, используется только неотслеживаемый запрос.</span><span class="sxs-lookup"><span data-stu-id="69474-161">The same as above, only use a non-tracking query.</span></span> <span data-ttu-id="69474-162">Это должно быть быстрее, так как разрешение идентификации не выполняется, и сущности не снапшоттед в целях отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="69474-162">This should be faster, since identity resolution isn't performed, and the entities aren't snapshotted for the purposes of change tracking.</span></span>
* <span data-ttu-id="69474-163">Старайтесь не загружать все экземпляры сущности блога, выполняя проецирование только ранжирования.</span><span class="sxs-lookup"><span data-stu-id="69474-163">Avoid loading the entire Blog entity instances at all, by projecting out the ranking only.</span></span> <span data-ttu-id="69474-164">Благодаря этому нам не нужно переносить другие ненужные столбцы типа сущности блога.</span><span class="sxs-lookup"><span data-stu-id="69474-164">The saves us from transferring the other, unneeded columns of the Blog entity type.</span></span>
* <span data-ttu-id="69474-165">Вычислите среднее значение в базе данных, сделав его частью запроса.</span><span class="sxs-lookup"><span data-stu-id="69474-165">Calculate the average in the database by making it part of the query.</span></span> <span data-ttu-id="69474-166">Это должен быть самый быстрый способ, так как все вычисляется в базе данных, и только результат передается обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="69474-166">This should be the fastest way, since everything is calculated in the database and only the result is transferred back to the client.</span></span>

<span data-ttu-id="69474-167">С помощью Бенчмаркдотнет вы пишете код для оценки производительности как простой метод, как и модульный тест, и Бенчмаркдотнет автоматически запускает каждый метод для достаточного количества итераций, надежно измеряя время и объем выделяемой памяти.</span><span class="sxs-lookup"><span data-stu-id="69474-167">With BenchmarkDotNet, you write the code to be benchmarked as a simple method - just like a unit test - and BenchmarkDotNet automatically runs each method for sufficient number of iterations, reliably measuring how long it takes and how much memory is allocated.</span></span> <span data-ttu-id="69474-168">Ниже приведен другой метод ([полный код теста производительности можно увидеть здесь](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Benchmarks/AverageBlogRanking.cs)):</span><span class="sxs-lookup"><span data-stu-id="69474-168">Here are the different method ([the full benchmark code can be seen here](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Benchmarks/AverageBlogRanking.cs)):</span></span>

### <a name="load-entities"></a>[<span data-ttu-id="69474-169">Загрузка сущностей</span><span class="sxs-lookup"><span data-stu-id="69474-169">Load entities</span></span>](#tab/load-entities)

[!code-csharp[Main](../../../samples/core/Benchmarks/AverageBlogRanking.cs?name=LoadEntities)]

### <a name="load-entities-no-tracking"></a>[<span data-ttu-id="69474-170">Загрузка сущностей, без отслеживания</span><span class="sxs-lookup"><span data-stu-id="69474-170">Load entities, no tracking</span></span>](#tab/load-entities-no-tracking)

[!code-csharp[Main](../../../samples/core/Benchmarks/AverageBlogRanking.cs?name=LoadEntitiesNoTracking)]

### <a name="project-only-ranking"></a>[<span data-ttu-id="69474-171">Ранжирование только по проекту</span><span class="sxs-lookup"><span data-stu-id="69474-171">Project only ranking</span></span>](#tab/project-only-ranking)

[!code-csharp[Main](../../../samples/core/Benchmarks/AverageBlogRanking.cs?name=ProjectOnlyRanking)]

### <a name="calculate-in-database"></a>[<span data-ttu-id="69474-172">Вычислить в базе данных</span><span class="sxs-lookup"><span data-stu-id="69474-172">Calculate in database</span></span>](#tab/calculate-in-database)

[!code-csharp[Main](../../../samples/core/Benchmarks/AverageBlogRanking.cs?name=CalculateInDatabase)]

***

<span data-ttu-id="69474-173">Ниже приведены результаты, выводимые Бенчмаркдотнет:</span><span class="sxs-lookup"><span data-stu-id="69474-173">The results are below, as printed by BenchmarkDotNet:</span></span>

|                 <span data-ttu-id="69474-174">Метод</span><span class="sxs-lookup"><span data-stu-id="69474-174">Method</span></span> |       <span data-ttu-id="69474-175">Среднее значение</span><span class="sxs-lookup"><span data-stu-id="69474-175">Mean</span></span> |    <span data-ttu-id="69474-176">Ошибка</span><span class="sxs-lookup"><span data-stu-id="69474-176">Error</span></span> |   <span data-ttu-id="69474-177">StdDev</span><span class="sxs-lookup"><span data-stu-id="69474-177">StdDev</span></span> |     <span data-ttu-id="69474-178">Median</span><span class="sxs-lookup"><span data-stu-id="69474-178">Median</span></span> | <span data-ttu-id="69474-179">Соотношение</span><span class="sxs-lookup"><span data-stu-id="69474-179">Ratio</span></span> | <span data-ttu-id="69474-180">ратиосд</span><span class="sxs-lookup"><span data-stu-id="69474-180">RatioSD</span></span> |    <span data-ttu-id="69474-181">Gen 0</span><span class="sxs-lookup"><span data-stu-id="69474-181">Gen 0</span></span> |   <span data-ttu-id="69474-182">Gen 1</span><span class="sxs-lookup"><span data-stu-id="69474-182">Gen 1</span></span> | <span data-ttu-id="69474-183">Gen 2</span><span class="sxs-lookup"><span data-stu-id="69474-183">Gen 2</span></span> |  <span data-ttu-id="69474-184">Allocated</span><span class="sxs-lookup"><span data-stu-id="69474-184">Allocated</span></span> |
|----------------------- |-----------:|---------:|---------:|-----------:|------:|--------:|---------:|--------:|------:|-----------:|
|           <span data-ttu-id="69474-185">лоадентитиес</span><span class="sxs-lookup"><span data-stu-id="69474-185">LoadEntities</span></span> | <span data-ttu-id="69474-186">2 860,4 США</span><span class="sxs-lookup"><span data-stu-id="69474-186">2,860.4 us</span></span> | <span data-ttu-id="69474-187">54,31 США</span><span class="sxs-lookup"><span data-stu-id="69474-187">54.31 us</span></span> | <span data-ttu-id="69474-188">93,68 США</span><span class="sxs-lookup"><span data-stu-id="69474-188">93.68 us</span></span> | <span data-ttu-id="69474-189">2 844,5 США</span><span class="sxs-lookup"><span data-stu-id="69474-189">2,844.5 us</span></span> |  <span data-ttu-id="69474-190">4.55</span><span class="sxs-lookup"><span data-stu-id="69474-190">4.55</span></span> |    <span data-ttu-id="69474-191">0,33</span><span class="sxs-lookup"><span data-stu-id="69474-191">0.33</span></span> | <span data-ttu-id="69474-192">210,9375</span><span class="sxs-lookup"><span data-stu-id="69474-192">210.9375</span></span> | <span data-ttu-id="69474-193">70,3125</span><span class="sxs-lookup"><span data-stu-id="69474-193">70.3125</span></span> |     - | <span data-ttu-id="69474-194">1309,56 КБ</span><span class="sxs-lookup"><span data-stu-id="69474-194">1309.56 KB</span></span> |
| <span data-ttu-id="69474-195">лоадентитиеснотраккинг</span><span class="sxs-lookup"><span data-stu-id="69474-195">LoadEntitiesNoTracking</span></span> | <span data-ttu-id="69474-196">1 353,0 США</span><span class="sxs-lookup"><span data-stu-id="69474-196">1,353.0 us</span></span> | <span data-ttu-id="69474-197">21,26 США</span><span class="sxs-lookup"><span data-stu-id="69474-197">21.26 us</span></span> | <span data-ttu-id="69474-198">18,85 США</span><span class="sxs-lookup"><span data-stu-id="69474-198">18.85 us</span></span> | <span data-ttu-id="69474-199">1 355,6 США</span><span class="sxs-lookup"><span data-stu-id="69474-199">1,355.6 us</span></span> |  <span data-ttu-id="69474-200">2.10</span><span class="sxs-lookup"><span data-stu-id="69474-200">2.10</span></span> |    <span data-ttu-id="69474-201">0,14</span><span class="sxs-lookup"><span data-stu-id="69474-201">0.14</span></span> |  <span data-ttu-id="69474-202">87,8906</span><span class="sxs-lookup"><span data-stu-id="69474-202">87.8906</span></span> |  <span data-ttu-id="69474-203">3,9063</span><span class="sxs-lookup"><span data-stu-id="69474-203">3.9063</span></span> |     - |  <span data-ttu-id="69474-204">540,09 КБ</span><span class="sxs-lookup"><span data-stu-id="69474-204">540.09 KB</span></span> |
|     <span data-ttu-id="69474-205">прожектонлиранкинг</span><span class="sxs-lookup"><span data-stu-id="69474-205">ProjectOnlyRanking</span></span> |   <span data-ttu-id="69474-206">910,9 США</span><span class="sxs-lookup"><span data-stu-id="69474-206">910.9 us</span></span> | <span data-ttu-id="69474-207">20,91 США</span><span class="sxs-lookup"><span data-stu-id="69474-207">20.91 us</span></span> | <span data-ttu-id="69474-208">61,65 США</span><span class="sxs-lookup"><span data-stu-id="69474-208">61.65 us</span></span> |   <span data-ttu-id="69474-209">892,9 США</span><span class="sxs-lookup"><span data-stu-id="69474-209">892.9 us</span></span> |  <span data-ttu-id="69474-210">1,46</span><span class="sxs-lookup"><span data-stu-id="69474-210">1.46</span></span> |    <span data-ttu-id="69474-211">0,14</span><span class="sxs-lookup"><span data-stu-id="69474-211">0.14</span></span> |  <span data-ttu-id="69474-212">41,0156</span><span class="sxs-lookup"><span data-stu-id="69474-212">41.0156</span></span> |  <span data-ttu-id="69474-213">0,9766</span><span class="sxs-lookup"><span data-stu-id="69474-213">0.9766</span></span> |     - |  <span data-ttu-id="69474-214">252,08 КБ</span><span class="sxs-lookup"><span data-stu-id="69474-214">252.08 KB</span></span> |
|    <span data-ttu-id="69474-215">калкулатеиндатабасе</span><span class="sxs-lookup"><span data-stu-id="69474-215">CalculateInDatabase</span></span> |   <span data-ttu-id="69474-216">627,1 США</span><span class="sxs-lookup"><span data-stu-id="69474-216">627.1 us</span></span> | <span data-ttu-id="69474-217">14,58 США</span><span class="sxs-lookup"><span data-stu-id="69474-217">14.58 us</span></span> | <span data-ttu-id="69474-218">42,54 США</span><span class="sxs-lookup"><span data-stu-id="69474-218">42.54 us</span></span> |   <span data-ttu-id="69474-219">626,4 США</span><span class="sxs-lookup"><span data-stu-id="69474-219">626.4 us</span></span> |  <span data-ttu-id="69474-220">1.00</span><span class="sxs-lookup"><span data-stu-id="69474-220">1.00</span></span> |    <span data-ttu-id="69474-221">0,00</span><span class="sxs-lookup"><span data-stu-id="69474-221">0.00</span></span> |   <span data-ttu-id="69474-222">4,8828</span><span class="sxs-lookup"><span data-stu-id="69474-222">4.8828</span></span> |       - |     - |   <span data-ttu-id="69474-223">33,27 КБ</span><span class="sxs-lookup"><span data-stu-id="69474-223">33.27 KB</span></span> |

> [!NOTE]
> <span data-ttu-id="69474-224">По мере того как методы создают и удаляют контекст в методе, эти операции учитываются для теста производительности, хотя и строго говоря, они не являются частью процесса запроса.</span><span class="sxs-lookup"><span data-stu-id="69474-224">As the methods instantiate and dispose the context within the method, these operations are counted for the benchmark, although strictly speaking they are not part of the querying process.</span></span> <span data-ttu-id="69474-225">Это не имеет значения, если цель состоит в сравнении двух альтернативных вариантов друг с другом (так как создание и реализация контекста одинаковы) и обеспечивает более целостное измерение для всей операции.</span><span class="sxs-lookup"><span data-stu-id="69474-225">This should not matter if the goal is to compare two alternatives to one another (since the context instantiation and disposal are the same), and gives a more holistic measurement for the entire operation.</span></span>

<span data-ttu-id="69474-226">Одно из ограничений Бенчмаркдотнет заключается в том, что оно измеряет простую, однопотоковую производительность предоставляемых методов и поэтому не подходит для параллельных сценариев.</span><span class="sxs-lookup"><span data-stu-id="69474-226">One limitation of BenchmarkDotNet is that it measures simple, single-thread performance of the methods you provide, and is therefore not well-suited for benchmarking concurrent scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69474-227">Всегда убедитесь, что в базе данных есть данные, аналогичные рабочим данным, при выполнении теста производительности; в противном случае результаты теста производительности могут не представлять фактическую производительность в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="69474-227">Always make sure to have data in your database that is similar to production data when benchmarking, otherwise the benchmark results may not represent actual performance in production.</span></span>