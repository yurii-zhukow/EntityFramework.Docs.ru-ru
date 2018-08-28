---
title: Подключение устойчивости и повторного выполнения логики - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 47181292873009c7bce2047787503258ffa35d9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997489"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="aa274-102">Логику устойчивости и повторите попытку подключения</span><span class="sxs-lookup"><span data-stu-id="aa274-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="aa274-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="aa274-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="aa274-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="aa274-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="aa274-105">Приложений, подключающихся к серверу базы данных всегда были уязвимы для разрывов соединений из-за внутренних сбоев и нестабильностью сети.</span><span class="sxs-lookup"><span data-stu-id="aa274-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="aa274-106">Тем не менее в среде локальной сети на основе работе для выделенной базы данных серверов эти ошибки можно достаточно редко, что дополнительную логику для обработки этих ошибок не часто требуется.</span><span class="sxs-lookup"><span data-stu-id="aa274-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="aa274-107">С ростом облачных основаны менее надежной сети, которые теперь чаще разрывы подключения серверов базы данных, таких как база данных SQL Windows Azure и подключения.</span><span class="sxs-lookup"><span data-stu-id="aa274-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="aa274-108">Это может быть вызвано защитных техник, облачной базы данных используют, чтобы обеспечить распределение ресурсов службы, например, регулирование подключения, либо к нестабильной работе в сети, приводит к времени ожидания периодических и других временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="aa274-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="aa274-109">Устойчивость подключений относится к возможности для Entity FRAMEWORK, автоматический повтор любых команд, которые не отвечают из-за этих разрывов соединения.</span><span class="sxs-lookup"><span data-stu-id="aa274-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="aa274-110">Стратегии выполнения</span><span class="sxs-lookup"><span data-stu-id="aa274-110">Execution Strategies</span></span>  

<span data-ttu-id="aa274-111">Повторную попытку подключения возложить на реализацию интерфейса IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="aa274-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="aa274-112">Реализации IDbExecutionStrategy будет отвечать за принятие операции и, при возникновении исключения, определение, если Повтор может быть выполнен и повторять их в случае.</span><span class="sxs-lookup"><span data-stu-id="aa274-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="aa274-113">Существуют четыре стратегии выполнения, входящие в состав EF.</span><span class="sxs-lookup"><span data-stu-id="aa274-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="aa274-114">**DefaultExecutionStrategy**: Эта стратегия выполнения не выполняет повторную попытку запуска любых операций, это значение по умолчанию для баз данных, отличных от sql server.</span><span class="sxs-lookup"><span data-stu-id="aa274-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="aa274-115">**DefaultSqlExecutionStrategy**: это стратегия внутреннего выполнения, которая используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa274-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="aa274-116">Эта стратегия не пытается повторно вообще, тем не менее, она будет разбита любые исключения, которые может быть временной для информирования пользователей, которые они хотели включить устойчивость подключения.</span><span class="sxs-lookup"><span data-stu-id="aa274-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="aa274-117">**DbExecutionStrategy**: этот класс подходит как базовый класс для других стратегий выполнения, включая собственные пользовательские.</span><span class="sxs-lookup"><span data-stu-id="aa274-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="aa274-118">Она реализует экспоненциальную политику повторов, где начальной повторная попытка происходит с нулевой задержки и задержка увеличивается экспоненциально пока не достигается максимальное число повторных попыток.</span><span class="sxs-lookup"><span data-stu-id="aa274-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="aa274-119">Этот класс содержит абстрактный метод ShouldRetryOn, который может быть реализован в производном выполнения стратегии для управления, какие исключения следует повторить.</span><span class="sxs-lookup"><span data-stu-id="aa274-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="aa274-120">**SqlAzureExecutionStrategy**: Эта стратегия выполнения наследует от DbExecutionStrategy и повторит попытку для исключений, которые заведомо возможно временных при работе с базой данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="aa274-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="aa274-121">Стратегии выполнения, 2 и 4, включаются в поставщике Sql Server, поставляемый с EF, которая находится в сборке EntityFramework.SqlServer и предназначены для работы с SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aa274-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="aa274-122">Включение стратегия выполнения</span><span class="sxs-lookup"><span data-stu-id="aa274-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="aa274-123">Самый простой способ сообщить EF для использования стратегии выполнения — с помощью метода SetExecutionStrategy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) класса:</span><span class="sxs-lookup"><span data-stu-id="aa274-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="aa274-124">Этот код указывает EF для использования SqlAzureExecutionStrategy при подключении к SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aa274-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="aa274-125">Настройка стратегии выполнения</span><span class="sxs-lookup"><span data-stu-id="aa274-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="aa274-126">Конструктор SqlAzureExecutionStrategy может принимать два параметра, значение параметра MaxRetryCount и MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="aa274-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="aa274-127">Число MaxRetry — это максимальное число попыток повтора стратегии.</span><span class="sxs-lookup"><span data-stu-id="aa274-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="aa274-128">MaxDelay — это интервал времени, представляющий максимальную задержку между повторными попытками, которые будут использовать стратегии выполнения.</span><span class="sxs-lookup"><span data-stu-id="aa274-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="aa274-129">Чтобы задать максимальное число повторных попыток 1 и Максимальная задержка до 30 секунд можно execue следующее:</span><span class="sxs-lookup"><span data-stu-id="aa274-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="aa274-130">SqlAzureExecutionStrategy повторит попытку мгновенной превышено в первый раз, временная ошибка возникает, но будет больше задержка между попытками, пока либо максимальное предельное число повторов или общее время достигает максимальный интервал.</span><span class="sxs-lookup"><span data-stu-id="aa274-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="aa274-131">Стратегии выполнения повторит только ограниченное число исключений, которые обычно являются tansient, по-прежнему необходимо будет обрабатывать другие ошибки, а также перехват исключения RetryLimitExceeded в случае когда ошибка не временная, или занимает слишком много времени для решения сам.</span><span class="sxs-lookup"><span data-stu-id="aa274-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="aa274-132">Ограничения</span><span class="sxs-lookup"><span data-stu-id="aa274-132">Limitations</span></span>  

<span data-ttu-id="aa274-133">Существуют некоторые известные ограничения при использовании стратегии выполнения повторной попытки:</span><span class="sxs-lookup"><span data-stu-id="aa274-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="aa274-134">Потоковой передачи запросы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="aa274-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="aa274-135">По умолчанию EF6 или более поздней версии будет буфера, результаты запроса, а не их streaming.</span><span class="sxs-lookup"><span data-stu-id="aa274-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="aa274-136">Если требуется, чтобы результаты потоковую передачу вы можно использовать метод AsStreaming изменение LINQ to Entities в запрос для потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="aa274-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="aa274-137">Не поддерживается потоковая передача, при регистрации стратегию выполнения повторной попытки.</span><span class="sxs-lookup"><span data-stu-id="aa274-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="aa274-138">Это ограничение существует, так как подключение удалось удалить части через возвращаемых результатов.</span><span class="sxs-lookup"><span data-stu-id="aa274-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="aa274-139">В этом случае EF, потребуется повторно выполнить весь запрос, но имеет надежного способа узнать, какие результаты уже были возвращены (данные могут измениться после начального запроса было отправлено, результаты могут вернуться в другом порядке, результаты могут не иметь уникальный идентификатор и т.д.).</span><span class="sxs-lookup"><span data-stu-id="aa274-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="aa274-140">Транзакции не поддерживаются, инициированное пользователем</span><span class="sxs-lookup"><span data-stu-id="aa274-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="aa274-141">Когда вы настроили стратегия выполнения, которая приводит к повторных попыток, существуют некоторые ограничения в использовании транзакций.</span><span class="sxs-lookup"><span data-stu-id="aa274-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="aa274-142">Поддерживаемые: платформы EF по умолчанию поведение транзакции</span><span class="sxs-lookup"><span data-stu-id="aa274-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="aa274-143">По умолчанию EF будет выполнять все обновления базы данных в рамках транзакции.</span><span class="sxs-lookup"><span data-stu-id="aa274-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="aa274-144">Не нужно ничего делать, чтобы включить эту, EF всегда выполняет это автоматически.</span><span class="sxs-lookup"><span data-stu-id="aa274-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="aa274-145">Например в следующем коде SaveChanges выполняется автоматически в рамках транзакции.</span><span class="sxs-lookup"><span data-stu-id="aa274-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="aa274-146">Если SaveChanges ошибкой после вставки одного нового узла, а затем был бы выполнен откат транзакции и никакие изменения не применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa274-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="aa274-147">Контекст также остается в состоянии, допускающем SaveChanges вызываться снова, чтобы повторить попытку применения изменений.</span><span class="sxs-lookup"><span data-stu-id="aa274-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="aa274-148">Что не поддерживается: транзакции, инициированное пользователем</span><span class="sxs-lookup"><span data-stu-id="aa274-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="aa274-149">Если не используется Повтор стратегию выполнения, можно включить несколько операций в рамках одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="aa274-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="aa274-150">Например следующий код создает оболочку двух вызовов SaveChanges в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="aa274-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="aa274-151">Если любой части либо операция завершается ошибкой затем изменения применяются.</span><span class="sxs-lookup"><span data-stu-id="aa274-151">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="aa274-152">Это не поддерживается при использовании стратегии выполнения повторной попытки, поскольку EF не будет учитывать любых предыдущих операций и как повторить их.</span><span class="sxs-lookup"><span data-stu-id="aa274-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="aa274-153">Например если второй SaveChanges неудачей, EF больше не имеет сведения, необходимые для повторите первый вызов метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="aa274-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="aa274-154">Возможные решения</span><span class="sxs-lookup"><span data-stu-id="aa274-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="aa274-155">Приостановка выполнения стратегии</span><span class="sxs-lookup"><span data-stu-id="aa274-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="aa274-156">Возможным решением является Приостановка Повтор стратегия выполнения для фрагмент программного кода, который должен использовать пользователь инициировал транзакции.</span><span class="sxs-lookup"><span data-stu-id="aa274-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="aa274-157">Самый простой способ сделать это является добавление флага SuspendExecutionStrategy в код на основе класса конфигурации и изменить лямбда-выражения стратегия выполнения для возврата стратегия выполнения по умолчанию (не retying), если флаг установлен.</span><span class="sxs-lookup"><span data-stu-id="aa274-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="aa274-158">Обратите внимание на то, что мы используем CallContext для хранения значения флага.</span><span class="sxs-lookup"><span data-stu-id="aa274-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="aa274-159">Это предоставляет аналогичную функциональность в локальное хранилище потока, но безопасен для использования асинхронного кода — включая асинхронный запрос и сохранить с Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa274-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="aa274-160">Теперь мы может приостановить стратегия выполнения для раздела кода, использующего инициированной пользователем транзакции.</span><span class="sxs-lookup"><span data-stu-id="aa274-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="aa274-161">Вручную вызвать стратегию выполнения</span><span class="sxs-lookup"><span data-stu-id="aa274-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="aa274-162">Другой вариант — вручную используйте стратегию выполнения и присвойте ему всего набора логики для выполнения, таким образом, чтобы его можно повторить все, что при сбое одной из операций.</span><span class="sxs-lookup"><span data-stu-id="aa274-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="aa274-163">Нам еще нужно приостановить стратегия выполнения — с применением технологии показанный выше -, так что любой контекст, используемые внутри блока кода повторной попытки не пытайтесь повторить попытку.</span><span class="sxs-lookup"><span data-stu-id="aa274-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="aa274-164">Обратите внимание на то, что все контексты должно быть создано в блоке кода, для повтора.</span><span class="sxs-lookup"><span data-stu-id="aa274-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="aa274-165">Это гарантирует, что мы начинаем с чистого состояния для каждой повторной попытки.</span><span class="sxs-lookup"><span data-stu-id="aa274-165">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

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

MyConfiguration.SuspendExecutionStrategy = false;
```  
