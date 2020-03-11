---
title: Устойчивость подключения — EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414137"
---
# <a name="connection-resiliency"></a><span data-ttu-id="077e0-102">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="077e0-102">Connection Resiliency</span></span>

<span data-ttu-id="077e0-103">Устойчивость подключений автоматически повторяет команды базы данных, завершившиеся сбоем.</span><span class="sxs-lookup"><span data-stu-id="077e0-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="077e0-104">Эту функцию можно использовать с любой базой данных, предоставив «стратегию выполнения», которая инкапсулирует логику, необходимую для обнаружения сбоев и команд повтора.</span><span class="sxs-lookup"><span data-stu-id="077e0-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="077e0-105">Поставщики EF Core могут предоставлять стратегии выполнения, связанные с конкретными условиями сбоя базы данных и оптимальными политиками повтора.</span><span class="sxs-lookup"><span data-stu-id="077e0-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="077e0-106">Например, поставщик SQL Server включает стратегию выполнения, специально адаптированную для SQL Server (включая SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="077e0-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="077e0-107">Он осведомлен о типах исключений, которые можно повторить, и имеет разумные значения по умолчанию для максимального количества повторных попыток, задержки между повторными попытками и т. д.</span><span class="sxs-lookup"><span data-stu-id="077e0-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="077e0-108">Стратегия выполнения указывается при настройке параметров для контекста.</span><span class="sxs-lookup"><span data-stu-id="077e0-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="077e0-109">Обычно это происходит в методе `OnConfiguring` производном контексте:</span><span class="sxs-lookup"><span data-stu-id="077e0-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="077e0-110">или в `Startup.cs` для ASP.NET Core приложения:</span><span class="sxs-lookup"><span data-stu-id="077e0-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="077e0-111">Настраиваемая стратегия выполнения</span><span class="sxs-lookup"><span data-stu-id="077e0-111">Custom execution strategy</span></span>

<span data-ttu-id="077e0-112">Существует механизм регистрации пользовательской стратегии выполнения, если вы хотите изменить какие-либо значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="077e0-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="077e0-113">Стратегии и транзакции выполнения</span><span class="sxs-lookup"><span data-stu-id="077e0-113">Execution strategies and transactions</span></span>

<span data-ttu-id="077e0-114">Стратегия выполнения, которая автоматически повторяет попытки сбоев, должна иметь возможность воспроизводить каждую операцию в блоке повтора, которая завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="077e0-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="077e0-115">Если повторные попытки включены, каждая операция, выполняемая с помощью EF Core, станет собственной операцией возможностью повторной попытки.</span><span class="sxs-lookup"><span data-stu-id="077e0-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="077e0-116">То есть каждый запрос и каждый вызов `SaveChanges()` будут повторены в качестве единицы при возникновении временного сбоя.</span><span class="sxs-lookup"><span data-stu-id="077e0-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="077e0-117">Однако, если код инициирует транзакцию с помощью `BeginTransaction()` вы определяете собственную группу операций, которую необходимо рассматривать как единую единицу, и все, что необходимо в рамках транзакции, должны быть воспроизведены, если это произойдет сбой.</span><span class="sxs-lookup"><span data-stu-id="077e0-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="077e0-118">При попытке сделать это при использовании стратегии выполнения вы получите исключение, аналогичное приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="077e0-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="077e0-119">InvalidOperationException: настроенная стратегия выполнения "Склсерверретринжексекутионстратеги" не поддерживает инициированные пользователем транзакции.</span><span class="sxs-lookup"><span data-stu-id="077e0-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="077e0-120">Используйте стратегию выполнения, возвращенную 'DbContext.Database.CreateExecutionStrategy()', чтобы выполнить все операции в транзакции как повторяемую единицу.</span><span class="sxs-lookup"><span data-stu-id="077e0-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="077e0-121">Решение заключается в том, чтобы вручную вызвать стратегию выполнения с делегатом, представляющим все, что необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="077e0-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="077e0-122">Если происходит временный сбой, стратегия выполнения снова вызывает делегат.</span><span class="sxs-lookup"><span data-stu-id="077e0-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="077e0-123">Этот подход также можно использовать с внешними транзакциями.</span><span class="sxs-lookup"><span data-stu-id="077e0-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="077e0-124">Сбой фиксации транзакции и проблема идемпотентности</span><span class="sxs-lookup"><span data-stu-id="077e0-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="077e0-125">Как правило, при сбое соединения выполняется откат текущей транзакции.</span><span class="sxs-lookup"><span data-stu-id="077e0-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="077e0-126">Однако, если соединение разорвано во время фиксации транзакции, результирующее состояние транзакции неизвестно.</span><span class="sxs-lookup"><span data-stu-id="077e0-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> 

<span data-ttu-id="077e0-127">По умолчанию стратегия выполнения повторяет операцию, как если бы был выполнен откат транзакции, но если это не так, это приведет к возникновению исключения, если новое состояние базы данных несовместимо или может привести к **повреждению данных** , если операция не зависит от определенного состояния, например при вставке новой строки с автоматически созданными значениями ключа.</span><span class="sxs-lookup"><span data-stu-id="077e0-127">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="077e0-128">Существует несколько способов решения этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="077e0-128">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="077e0-129">Вариант 1-Do (почти) Nothing</span><span class="sxs-lookup"><span data-stu-id="077e0-129">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="077e0-130">Вероятность сбоя подключения во время фиксации транзакции мала, поэтому приложение может быть вполне неудачным, если это условие действительно происходит.</span><span class="sxs-lookup"><span data-stu-id="077e0-130">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="077e0-131">Однако необходимо избегать использования созданных хранилищем ключей, чтобы гарантировать, что вместо добавления повторяющейся строки создается исключение.</span><span class="sxs-lookup"><span data-stu-id="077e0-131">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="077e0-132">Рассмотрите возможность использования генерируемого клиентом значения GUID или генератора значений на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="077e0-132">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="077e0-133">Вариант 2. Перестроение состояния приложения</span><span class="sxs-lookup"><span data-stu-id="077e0-133">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="077e0-134">Отменить текущую `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="077e0-134">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="077e0-135">Создайте новый `DbContext` и восстановите состояние приложения из базы данных.</span><span class="sxs-lookup"><span data-stu-id="077e0-135">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="077e0-136">Сообщите пользователю о том, что последняя операция могла завершиться неудачно.</span><span class="sxs-lookup"><span data-stu-id="077e0-136">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="077e0-137">Вариант 3. Проверка добавления состояния</span><span class="sxs-lookup"><span data-stu-id="077e0-137">Option 3 - Add state verification</span></span>

<span data-ttu-id="077e0-138">Для большинства операций, изменяющих состояние базы данных, можно добавить код, который проверяет, была ли она успешной.</span><span class="sxs-lookup"><span data-stu-id="077e0-138">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="077e0-139">EF предоставляет метод расширения для упрощения `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="077e0-139">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="077e0-140">Этот метод начинает и фиксирует транзакцию, а также принимает функцию в `verifySucceeded` параметр, который вызывается при возникновении временной ошибки во время фиксации транзакции.</span><span class="sxs-lookup"><span data-stu-id="077e0-140">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="077e0-141">Здесь `SaveChanges` вызывается с `acceptAllChangesOnSuccess` установленным в значение `false`, чтобы избежать изменения состояния сущности `Blog` на `Unchanged`, если `SaveChanges` выполняется.</span><span class="sxs-lookup"><span data-stu-id="077e0-141">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="077e0-142">Это позволяет повторить ту же операцию в случае сбоя фиксации и отката транзакции.</span><span class="sxs-lookup"><span data-stu-id="077e0-142">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="077e0-143">Вариант 4. Отслеживание транзакции вручную</span><span class="sxs-lookup"><span data-stu-id="077e0-143">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="077e0-144">Если необходимо использовать созданные хранилищем ключи или требуется универсальный способ обработки ошибок фиксации, которые не зависят от выполняемой операции, можно назначить идентификатор, который проверяется при сбое фиксации.</span><span class="sxs-lookup"><span data-stu-id="077e0-144">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="077e0-145">Добавьте таблицу в базу данных, используемую для наблюдения за состоянием транзакций.</span><span class="sxs-lookup"><span data-stu-id="077e0-145">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="077e0-146">Вставка строки в таблицу в начале каждой транзакции.</span><span class="sxs-lookup"><span data-stu-id="077e0-146">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="077e0-147">В случае сбоя соединения во время фиксации проверьте наличие соответствующей строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="077e0-147">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="077e0-148">Если фиксация прошла успешно, удалите соответствующую строку, чтобы избежать роста таблицы.</span><span class="sxs-lookup"><span data-stu-id="077e0-148">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="077e0-149">Убедитесь, что контекст, используемый для проверки, имеет стратегию выполнения, определенную, так как соединение, скорее всего, снова завершится сбоем во время проверки, если оно завершилось сбоем во время фиксации транзакции.</span><span class="sxs-lookup"><span data-stu-id="077e0-149">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
