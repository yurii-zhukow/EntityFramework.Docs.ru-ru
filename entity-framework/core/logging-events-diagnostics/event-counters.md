---
title: Счетчики событий — EF Core
description: Отслеживание EF Core производительности и диагностика аномалий с помощью счетчиков событий .NET
author: roji
ms.date: 11/17/2020
uid: core/logging-events-diagnostics/event-counters
ms.openlocfilehash: 46acfe82d8aeb7d16146bae0cc2cd4ff733e2831
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003693"
---
# <a name="event-counters"></a><span data-ttu-id="8d033-103">Счетчики событий</span><span class="sxs-lookup"><span data-stu-id="8d033-103">Event Counters</span></span>

> [!NOTE]
> <span data-ttu-id="8d033-104">Эта функция была добавлена в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="8d033-104">This feature was added in EF Core 5.0.</span></span>

<span data-ttu-id="8d033-105">Entity Framework Core (EF Core) предоставляет непрерывные числовые метрики, которые могут предоставить хорошее указание о работоспособности программы.</span><span class="sxs-lookup"><span data-stu-id="8d033-105">Entity Framework Core (EF Core) exposes continuous numeric metrics which can provide a good indication of your program's health.</span></span> <span data-ttu-id="8d033-106">Эти метрики можно использовать в следующих целях:</span><span class="sxs-lookup"><span data-stu-id="8d033-106">These metrics can be used for the following purposes:</span></span>

* <span data-ttu-id="8d033-107">Отследите общую нагрузку базы данных в режиме реального времени, так как приложение выполняется</span><span class="sxs-lookup"><span data-stu-id="8d033-107">Track general database load in realtime as the application is running</span></span>
* <span data-ttu-id="8d033-108">Раскрывайте проблемные методики программирования, которые могут привести к снижению производительности</span><span class="sxs-lookup"><span data-stu-id="8d033-108">Expose problematic coding practices which can lead to degraded performance</span></span>
* <span data-ttu-id="8d033-109">Мониторинг и изолирование аномального поведения программы</span><span class="sxs-lookup"><span data-stu-id="8d033-109">Track down and isolate anomalous program behavior</span></span>

<span data-ttu-id="8d033-110">EF Core отчеты метрик с помощью стандартной функции счетчиков событий .NET; рекомендуется прочитать [эту запись блога](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0/) для быстрого обзора работы счетчиков.</span><span class="sxs-lookup"><span data-stu-id="8d033-110">EF Core reports metrics via the standard .NET event counters feature; it's recommended to read [this blog post](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0/) for a quick overview of how counters work.</span></span>

## <a name="attach-to-a-process-using-dotnet-counters"></a><span data-ttu-id="8d033-111">Присоединение к процессу с помощью DotNet-Counters</span><span class="sxs-lookup"><span data-stu-id="8d033-111">Attach to a process using dotnet-counters</span></span>

<span data-ttu-id="8d033-112">[Инструмент DotNet-Counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) можно использовать для одновременного присоединения к выполняющемуся процессу и отчетов EF Core счетчиков событий. чтобы эти счетчики были доступны, в программе ничего делать не нужно.</span><span class="sxs-lookup"><span data-stu-id="8d033-112">The [dotnet-counters tool](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) can be used to attach to a running process and report EF Core event counters regularly; nothing special needs to be done in the program for these counters to be available.</span></span>

<span data-ttu-id="8d033-113">Сначала установите `dotnet-counters` средство: `dotnet tool install --global dotnet-counters` .</span><span class="sxs-lookup"><span data-stu-id="8d033-113">First, install the `dotnet-counters` tool: `dotnet tool install --global dotnet-counters`.</span></span>

<span data-ttu-id="8d033-114">Затем найдите идентификатор процесса (PID) процесса .NET, выполняющего приложение EF Core:</span><span class="sxs-lookup"><span data-stu-id="8d033-114">Next, find the process ID (PID) of the .NET process running your EF Core application:</span></span>

### <a name="windows"></a>[<span data-ttu-id="8d033-115">Windows</span><span class="sxs-lookup"><span data-stu-id="8d033-115">Windows</span></span>](#tab/windows)

1. <span data-ttu-id="8d033-116">Откройте диспетчер задач Windows, щелкнув правой кнопкой мыши панель задач и выбрав пункт "Диспетчер задач".</span><span class="sxs-lookup"><span data-stu-id="8d033-116">Open the Windows Task Manager by right-clicking on the task bar and selecting "Task Manager".</span></span>
2. <span data-ttu-id="8d033-117">Убедитесь, что в нижней части окна выбран параметр "Дополнительные сведения".</span><span class="sxs-lookup"><span data-stu-id="8d033-117">Make sure that the "More details" option is selected at the bottom of the window.</span></span>
3. <span data-ttu-id="8d033-118">На вкладке процессы щелкните правой кнопкой мыши столбец и убедитесь, что столбец PID включен.</span><span class="sxs-lookup"><span data-stu-id="8d033-118">In the Processes tab, right-click a column and make sure that the PID column is enabled.</span></span>
4. <span data-ttu-id="8d033-119">В списке процесс выберите приложение и получите идентификатор его процесса из столбца PID.</span><span class="sxs-lookup"><span data-stu-id="8d033-119">Locate your application in the process list, and get its process ID from the PID column.</span></span>

### <a name="linux-or-macos"></a>[<span data-ttu-id="8d033-120">Linux и macOS</span><span class="sxs-lookup"><span data-stu-id="8d033-120">Linux or macOS</span></span>](#tab/fluent-api)

1. <span data-ttu-id="8d033-121">Используйте `ps` команду, чтобы вывести список всех процессов, выполняемых для пользователя.</span><span class="sxs-lookup"><span data-stu-id="8d033-121">Use the `ps` command to list all processes running for your user.</span></span>
2. <span data-ttu-id="8d033-122">В списке процесс выберите приложение и получите идентификатор его процесса из столбца PID.</span><span class="sxs-lookup"><span data-stu-id="8d033-122">Locate your application in the process list, and get its process ID from the PID column.</span></span>

***

<span data-ttu-id="8d033-123">Внутри приложения .NET идентификатор процесса доступен как `Process.GetCurrentProcess().Id` ; это может быть полезно для печати PID при запуске.</span><span class="sxs-lookup"><span data-stu-id="8d033-123">Inside your .NET application, the process ID is available as `Process.GetCurrentProcess().Id`; this can be useful for printing the PID upon startup.</span></span>

<span data-ttu-id="8d033-124">Наконец, запустите программу `dotnet-counters` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8d033-124">Finally, launch `dotnet-counters` as follows:</span></span>

```console
dotnet counters monitor Microsoft.EntityFrameworkCore -p <PID>
```

<span data-ttu-id="8d033-125">`dotnet-counters` Теперь подключается к выполняющемуся процессу и запускает отчет о данных непрерывных счетчиков:</span><span class="sxs-lookup"><span data-stu-id="8d033-125">`dotnet-counters` will now attach to your running process and start reporting continuous counter data:</span></span>

```console
Press p to pause, r to resume, q to quit.
 Status: Running

[Microsoft.EntityFrameworkCore]
    Active DbContexts                                               1
    Execution Strategy Operation Failures (Count / 1 sec)           0
    Execution Strategy Operation Failures (Total)                   0
    Optimistic Concurrency Failures (Count / 1 sec)                 0
    Optimistic Concurrency Failures (Total)                         0
    Queries (Count / 1 sec)                                         1
    Queries (Total)                                               189
    Query Cache Hit Rate (%)                                      100
    SaveChanges (Count / 1 sec)                                     0
    SaveChanges (Total)                                             0
```

## <a name="counters-and-their-meaning"></a><span data-ttu-id="8d033-126">Счетчики и их значения</span><span class="sxs-lookup"><span data-stu-id="8d033-126">Counters and their meaning</span></span>

<span data-ttu-id="8d033-127">Имя счетчика</span><span class="sxs-lookup"><span data-stu-id="8d033-127">Counter name</span></span>                          | <span data-ttu-id="8d033-128">Описание</span><span class="sxs-lookup"><span data-stu-id="8d033-128">Description</span></span>
------------------------------------- | ----
<span data-ttu-id="8d033-129">Активные DbContext</span><span class="sxs-lookup"><span data-stu-id="8d033-129">Active DbContexts</span></span>                     | <span data-ttu-id="8d033-130">Число активных, неуничтоженных экземпляров DbContext, в данный момент находящихся в приложении.</span><span class="sxs-lookup"><span data-stu-id="8d033-130">The number of active, undisposed DbContext instances currently in your application.</span></span> <span data-ttu-id="8d033-131">Если это число постоянно растет, может возникнуть утечка из-за неправильного освобождения экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="8d033-131">If this number grows continuously, you may have a leak because DbContext instances aren't being properly disposed.</span></span> <span data-ttu-id="8d033-132">Обратите внимание, что если включена [Группировка пула](xref:core/miscellaneous/context-pooling) , это число включает в себя экземпляры DbContext в пуле, которые в настоящее время не используются.</span><span class="sxs-lookup"><span data-stu-id="8d033-132">Note that if [context pooling](xref:core/miscellaneous/context-pooling) is enabled, this number includes pooled DbContext instances not currently in use.</span></span>
<span data-ttu-id="8d033-133">Сбои операций стратегии выполнения</span><span class="sxs-lookup"><span data-stu-id="8d033-133">Execution Strategy Operation Failures</span></span> | <span data-ttu-id="8d033-134">Количество раз, когда не удалось выполнить операцию базы данных.</span><span class="sxs-lookup"><span data-stu-id="8d033-134">The number of times a database operation failed to execute.</span></span> <span data-ttu-id="8d033-135">Если включена стратегия повторного выполнения, она включает в себя каждый отдельный сбой в рамках нескольких попыток выполнения одной операции.</span><span class="sxs-lookup"><span data-stu-id="8d033-135">If a retrying execution strategy is enabled, this includes each individual failure within multiple attempts on the same operation.</span></span> <span data-ttu-id="8d033-136">Это можно использовать для обнаружения временных проблем с инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="8d033-136">This can be used to detect transient issues with your infrastructure.</span></span>
<span data-ttu-id="8d033-137">Сбои оптимистичного параллелизма</span><span class="sxs-lookup"><span data-stu-id="8d033-137">Optimistic Concurrency Failures</span></span>       | <span data-ttu-id="8d033-138">Количество сбоев из- `SaveChanges` за ошибки оптимистического параллелизма, так как данные в хранилище данных были изменены после его загрузки.</span><span class="sxs-lookup"><span data-stu-id="8d033-138">The number of times `SaveChanges` failed because of an optimistic concurrency error, because data in the data store was changed since your code loaded it.</span></span> <span data-ttu-id="8d033-139">Это соответствует <xref:Microsoft.EntityFrameworkCore.DbUpdateConcurrencyException> вызываемому объекту.</span><span class="sxs-lookup"><span data-stu-id="8d033-139">This corresponds to a <xref:Microsoft.EntityFrameworkCore.DbUpdateConcurrencyException> being thrown.</span></span>
<span data-ttu-id="8d033-140">Запросы</span><span class="sxs-lookup"><span data-stu-id="8d033-140">Queries</span></span>                               | <span data-ttu-id="8d033-141">Число выполненных запросов.</span><span class="sxs-lookup"><span data-stu-id="8d033-141">The number of queries executed.</span></span>
<span data-ttu-id="8d033-142">Частота попаданий в кэш запросов (%)</span><span class="sxs-lookup"><span data-stu-id="8d033-142">Query Cache Hit Rate (%)</span></span>              | <span data-ttu-id="8d033-143">Отношение попаданий в кэш запросов к промахам.</span><span class="sxs-lookup"><span data-stu-id="8d033-143">The ratio of query cache hits to misses.</span></span> <span data-ttu-id="8d033-144">При первом выполнении запроса LINQ с помощью EF Core (исключая параметры) он должен быть скомпилирован в том, что является относительно интенсивным процессом.</span><span class="sxs-lookup"><span data-stu-id="8d033-144">The first time a given LINQ query is executed by EF Core (excluding parameters), it must be compiled in what is a relatively heavy process.</span></span> <span data-ttu-id="8d033-145">В обычном приложении все запросы используются повторно, а частота попаданий кэша запросов должна быть стабильной на 100% после начального периода прогрева.</span><span class="sxs-lookup"><span data-stu-id="8d033-145">In a normal application, all queries are reused, and the query cache hit rate should be stable at 100% after an initial warmup period.</span></span> <span data-ttu-id="8d033-146">Если это число менее 100% со временем, может произойти снижение производительности из-за повторных компиляций, что может быть результатом неоптимального создания динамического запроса.</span><span class="sxs-lookup"><span data-stu-id="8d033-146">If this number is less than 100% over time, you may experience degraded perf due to repeated compilations, which could be a result of suboptimal dynamic query generation.</span></span>
<span data-ttu-id="8d033-147">Сохранение изменений</span><span class="sxs-lookup"><span data-stu-id="8d033-147">SaveChanges</span></span>                           | <span data-ttu-id="8d033-148">Число вызовов `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="8d033-148">The number of times `SaveChanges` has been called.</span></span> <span data-ttu-id="8d033-149">Обратите внимание, что `SaveChanges` сохраняет несколько изменений в одном пакете, поэтому это не обязательно представляет каждое отдельное обновление, выполненное в одной сущности.</span><span class="sxs-lookup"><span data-stu-id="8d033-149">Note that `SaveChanges` saves multiple changes in a single batch, so this doesn't necessarily represent each individual update done on a single entity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d033-150">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8d033-150">Additional resources</span></span>

* [<span data-ttu-id="8d033-151">Документация .NET по счетчикам событий</span><span class="sxs-lookup"><span data-stu-id="8d033-151">.NET documentation on event counters</span></span>](https://docs.microsoft.com/dotnet/core/diagnostics/event-counters)
