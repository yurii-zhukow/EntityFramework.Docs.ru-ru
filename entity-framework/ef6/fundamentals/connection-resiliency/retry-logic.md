---
title: Отказоустойчивость подключения и логика повторных попыток — EF6
description: Отказоустойчивость подключения и логика повторных попыток в Entity Framework 6
author: AndriySvyryd
ms.date: 11/20/2019
uid: ef6/fundamentals/connection-resiliency/retry-logic
ms.openlocfilehash: 3e71e973be5a23c86f873e9adf1bf00f4b6def58
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073192"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="54e85-103">Отказоустойчивость подключения и логика повторных попыток</span><span class="sxs-lookup"><span data-stu-id="54e85-103">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="54e85-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="54e85-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="54e85-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="54e85-105">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="54e85-106">Приложения, подключающиеся к серверу базы данных, всегда были уязвимы для разрывов соединения из-за сбоев серверной части и нестабильности сети.</span><span class="sxs-lookup"><span data-stu-id="54e85-106">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="54e85-107">Однако в среде на основе локальной сети, работающей на выделенных серверах баз данных, эти ошибки достаточно редки, так что дополнительные алгоритмы обработки этих сбоев часто не требуются.</span><span class="sxs-lookup"><span data-stu-id="54e85-107">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="54e85-108">При игре на облачные серверы баз данных, такие как база данных SQL Windows Azure и подключения к менее надежным сетям, теперь становится наиболее распространенным нарушением подключения.</span><span class="sxs-lookup"><span data-stu-id="54e85-108">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="54e85-109">Это может быть вызвано защитой методик, которые используются в облачных базах данных для обеспечения равномерного распределения обслуживания, например регулирования подключений, а также для нестабильной работы сети, в результате которой возникают периодические тайм-ауты и другие временные ошибки.</span><span class="sxs-lookup"><span data-stu-id="54e85-109">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="54e85-110">Устойчивость подключения относится к возможности EF автоматически повторять команды, которые не удалось выполнить из-за этих разрывов соединения.</span><span class="sxs-lookup"><span data-stu-id="54e85-110">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="54e85-111">Стратегии выполнения</span><span class="sxs-lookup"><span data-stu-id="54e85-111">Execution Strategies</span></span>  

<span data-ttu-id="54e85-112">Повтор соединения осуществляется с помощью реализации интерфейса IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="54e85-112">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="54e85-113">Реализации IDbExecutionStrategy будут отвечать за принятие операции и, при возникновении исключения, определить, подходит ли повторная попытка, и повторить попытку, если это так.</span><span class="sxs-lookup"><span data-stu-id="54e85-113">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="54e85-114">Существует четыре стратегии выполнения, поставляемые с EF:</span><span class="sxs-lookup"><span data-stu-id="54e85-114">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="54e85-115">**Дефаултексекутионстратеги**: Эта стратегия выполнения не повторяет никаких операций, она используется по умолчанию для баз данных, отличных от SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54e85-115">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="54e85-116">**DefaultSqlExecutionStrategy**: это внутренняя стратегия выполнения, используемая по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54e85-116">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="54e85-117">Эта стратегия не выполняет повторную попытку, однако она будет переносить любые исключения, которые могут быть временными, чтобы информировать пользователей о том, что им может потребоваться обеспечить устойчивость подключения.</span><span class="sxs-lookup"><span data-stu-id="54e85-117">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="54e85-118">**Дбексекутионстратеги**. Этот класс подходит как базовый класс для других стратегий выполнения, включая собственные пользовательские.</span><span class="sxs-lookup"><span data-stu-id="54e85-118">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="54e85-119">Он реализует экспоненциальную политику повтора, в которой происходит начальная повторная попытка с нулевой задержкой, а задержка увеличивается экспоненциально до тех пор, пока не будет достигнуто максимальное число повторных попыток.</span><span class="sxs-lookup"><span data-stu-id="54e85-119">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="54e85-120">Этот класс имеет абстрактный метод Шаулдретрйон, который можно реализовать в стратегиях выполнения, чтобы управлять тем, какие исключения следует повторить.</span><span class="sxs-lookup"><span data-stu-id="54e85-120">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="54e85-121">**SqlAzureExecutionStrategy**: Эта стратегия выполнения наследуется от дбексекутионстратеги и будет повторяться при возникновении исключений, которые, возможно, будут временными при работе с базой данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="54e85-121">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="54e85-122">Стратегии выполнения 2 и 4 включены в поставщик SQL Server, поставляемый с EF, который находится в сборке EntityFramework. SqlServer и предназначен для работы с SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54e85-122">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="54e85-123">Включение стратегии выполнения</span><span class="sxs-lookup"><span data-stu-id="54e85-123">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="54e85-124">Самый простой способ указать EF использовать стратегию выполнения — с помощью метода Сетексекутионстратеги класса [DbConfiguration](xref:ef6/fundamentals/configuring/code-based) :</span><span class="sxs-lookup"><span data-stu-id="54e85-124">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](xref:ef6/fundamentals/configuring/code-based) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="54e85-125">Этот код указывает EF использовать SqlAzureExecutionStrategy при соединении с SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54e85-125">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="54e85-126">Настройка стратегии выполнения</span><span class="sxs-lookup"><span data-stu-id="54e85-126">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="54e85-127">Конструктор SqlAzureExecutionStrategy может принимать два параметра: Максретрикаунт и Максделай.</span><span class="sxs-lookup"><span data-stu-id="54e85-127">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="54e85-128">MaxRetry Count — это максимальное количество повторных попыток стратегии.</span><span class="sxs-lookup"><span data-stu-id="54e85-128">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="54e85-129">Максделай — это интервал времени, представляющий максимальную задержку между повторными попытками, которые будет использовать стратегия выполнения.</span><span class="sxs-lookup"><span data-stu-id="54e85-129">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="54e85-130">Чтобы задать максимальное число повторных попыток до 1 и максимальную задержку в 30 секунд, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="54e85-130">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="54e85-131">SqlAzureExecutionStrategy повторит попытку в первый раз, когда произойдет временной сбой, но задерживается дольше между повторными попытками до тех пор, пока не будет превышено максимальное число повторных попыток или общее время достигнет максимальной задержки.</span><span class="sxs-lookup"><span data-stu-id="54e85-131">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="54e85-132">Стратегии выполнения будут пытаться только ограничить количество исключений, которые обычно являются временными, но все равно потребуется обработать другие ошибки, а также перехватывать исключение Ретрилимитексцеедед в случае, когда ошибка не является временной или занимает слишком много времени для разрешения.</span><span class="sxs-lookup"><span data-stu-id="54e85-132">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="54e85-133">Существует ряд известных ограничений при использовании стратегии повторного выполнения.</span><span class="sxs-lookup"><span data-stu-id="54e85-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="54e85-134">Потоковые запросы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="54e85-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="54e85-135">По умолчанию EF6 и более поздние версии будут помещает результаты запроса в буфер, а не потоки.</span><span class="sxs-lookup"><span data-stu-id="54e85-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="54e85-136">Если требуется потоковая передача результатов, можно использовать метод Асстреаминг для изменения LINQ to Entities запроса для потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="54e85-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="54e85-137">Потоковая передача не поддерживается при регистрации стратегии повторного выполнения.</span><span class="sxs-lookup"><span data-stu-id="54e85-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="54e85-138">Это ограничение связано с тем, что подключение может частично отброситься через возвращаемые результаты.</span><span class="sxs-lookup"><span data-stu-id="54e85-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="54e85-139">В этом случае EF необходимо повторно запустить весь запрос, но не имеет надежного способа узнать, какие результаты уже были возвращены (данные могли быть изменены с момента отправки первоначального запроса, результаты могут возвращаться в другом порядке, результаты могут не иметь уникальных идентификаторов и т. д.).</span><span class="sxs-lookup"><span data-stu-id="54e85-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="54e85-140">Транзакции, инициированные пользователем, не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="54e85-140">User initiated transactions are not supported</span></span>  

<span data-ttu-id="54e85-141">После настройки стратегии выполнения, которая приводит к повторным попыткам, существуют некоторые ограничения на использование транзакций.</span><span class="sxs-lookup"><span data-stu-id="54e85-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="54e85-142">По умолчанию EF выполняет любые обновления базы данных в рамках транзакции.</span><span class="sxs-lookup"><span data-stu-id="54e85-142">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="54e85-143">Для этого не нужно ничего делать, так как EF всегда делает это автоматически.</span><span class="sxs-lookup"><span data-stu-id="54e85-143">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="54e85-144">Например, в следующем коде SaveChanges автоматически выполняется в рамках транзакции.</span><span class="sxs-lookup"><span data-stu-id="54e85-144">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="54e85-145">Если при вставке одного из новых сайтов вызов SaveChanges завершился сбоем, то будет выполнен откат транзакции, и изменения не будут применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="54e85-145">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="54e85-146">Контекст также остается в состоянии, которое позволяет повторно вызывать SaveChanges для повторного применения изменений.</span><span class="sxs-lookup"><span data-stu-id="54e85-146">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="54e85-147">Если стратегия повторного выполнения не используется, можно заключить несколько операций в одну транзакцию.</span><span class="sxs-lookup"><span data-stu-id="54e85-147">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="54e85-148">Например, следующий код создает оболочку для двух вызовов SaveChanges в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="54e85-148">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="54e85-149">Если какая либо часть операции завершается ошибкой, никакие изменения не применяются.</span><span class="sxs-lookup"><span data-stu-id="54e85-149">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="54e85-150">Это не поддерживается при использовании стратегии повторного выполнения, поскольку EF не знает о каких-либо предыдущих операциях и способах их повторить.</span><span class="sxs-lookup"><span data-stu-id="54e85-150">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="54e85-151">Например, если произошел сбой во втором вызове SaveChanges, то EF больше не будет иметь требуемых сведений для повторного выполнения первого вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="54e85-151">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="54e85-152">Решение. Стратегия выполнения вызова вручную</span><span class="sxs-lookup"><span data-stu-id="54e85-152">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="54e85-153">Решение состоит в том, чтобы вручную использовать стратегию выполнения и дать ей весь набор логики для выполнения, чтобы он мог повторить все, если одна из операций завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="54e85-153">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="54e85-154">При выполнении стратегии выполнения, производной от Дбексекутионстратеги, она приостанавливает неявную стратегию выполнения, используемую в SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="54e85-154">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="54e85-155">Обратите внимание, что все контексты должны быть созданы в блоке кода для повторной попытки.</span><span class="sxs-lookup"><span data-stu-id="54e85-155">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="54e85-156">Это гарантирует, что мы начинаем с чистого состояния для каждой повторной попытки.</span><span class="sxs-lookup"><span data-stu-id="54e85-156">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });
```  
