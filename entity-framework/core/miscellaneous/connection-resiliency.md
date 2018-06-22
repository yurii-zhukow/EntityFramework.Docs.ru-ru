---
title: Устойчивость подключений - EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053534"
---
# <a name="connection-resiliency"></a><span data-ttu-id="a5050-102">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="a5050-102">Connection Resiliency</span></span>

<span data-ttu-id="a5050-103">Устойчивость подключения автоматически повторяет команды поврежденной базы данных.</span><span class="sxs-lookup"><span data-stu-id="a5050-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="a5050-104">Компонент может использоваться с любой базой данных, указав «стратегия выполнения», который инкапсулирует логику, необходимую для обнаружения сбоев и повторите команды.</span><span class="sxs-lookup"><span data-stu-id="a5050-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="a5050-105">EF основные поставщики могут предоставлять точном соответствии с их условия сбоя для заданной базы данных и политики повтора оптимальной стратегии выполнения.</span><span class="sxs-lookup"><span data-stu-id="a5050-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="a5050-106">Например поставщик SQL Server включает в себя стратегия выполнения, которая специально предназначено для SQL Server (включая SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="a5050-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="a5050-107">Он учитывает типов исключений, которые могут быть повторены и имеет допустимых значений по умолчанию для максимального количества повторных попыток, задержка между повторными попытками и т. д.</span><span class="sxs-lookup"><span data-stu-id="a5050-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="a5050-108">Стратегия выполнения задается при настройке параметров для текущего контекста.</span><span class="sxs-lookup"><span data-stu-id="a5050-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="a5050-109">Это обычно находится в `OnConfiguring` метода производного контексте или в `Startup.cs` для приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5050-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="a5050-110">Стратегия выполнения пользовательских</span><span class="sxs-lookup"><span data-stu-id="a5050-110">Custom execution strategy</span></span>

<span data-ttu-id="a5050-111">Нет механизма регистрации стратегии выполнения пользовательских своим собственным в том случае, если вы хотите изменить любые параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a5050-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="a5050-112">Стратегиями выполнения и транзакции</span><span class="sxs-lookup"><span data-stu-id="a5050-112">Execution strategies and transactions</span></span>

<span data-ttu-id="a5050-113">Стратегия выполнения, которая автоматически предпринимает попытки восстановления при сбое необходимо воспроизвести каждой операции в блоке повторных попыток, завершаются с ошибкой.</span><span class="sxs-lookup"><span data-stu-id="a5050-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="a5050-114">После включения повторных попыток, каждой операции, выполняемые через основной EF становится отдельной возможностью повторной попытки операции, т. е. каждый запрос и каждый вызов `SaveChanges()` будет повторена как единое целое, если возникает временная ошибка.</span><span class="sxs-lookup"><span data-stu-id="a5050-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="a5050-115">Тем не менее если код инициирует транзакцию, используя `BeginTransaction()` вы определяете собственную группу операций, которые должны рассматриваться как единое целое, т. е. весь код внутри транзакции необходимо воспроизводить должен произойти сбой.</span><span class="sxs-lookup"><span data-stu-id="a5050-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="a5050-116">При попытке сделать это при использовании стратегия выполнения, будет получено исключение следующим образом.</span><span class="sxs-lookup"><span data-stu-id="a5050-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="a5050-117">Исключение InvalidOperationException: Настроенная стратегия выполнения «SqlServerRetryingExecutionStrategy» не поддерживает транзакции, инициированной пользователем.</span><span class="sxs-lookup"><span data-stu-id="a5050-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="a5050-118">Используйте возвращенный «DbContext.Database.CreateExecutionStrategy()» стратегии выполнения для выполнения всех операций в транзакции как единое возможностью повторной попытки.</span><span class="sxs-lookup"><span data-stu-id="a5050-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="a5050-119">Решением является вызвать стратегия выполнения отражающее все делегатом, который должен выполняться вручную.</span><span class="sxs-lookup"><span data-stu-id="a5050-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="a5050-120">В случае временного сбоя стратегия выполнения будет снова вызвать делегата.</span><span class="sxs-lookup"><span data-stu-id="a5050-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="a5050-121">Ошибка фиксации транзакции и о проблеме идемпотентности</span><span class="sxs-lookup"><span data-stu-id="a5050-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="a5050-122">В общем случае при сбое подключения текущая транзакция откатывается.</span><span class="sxs-lookup"><span data-stu-id="a5050-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="a5050-123">Тем не менее, если соединение разорвано во время транзакции выполняется зафиксирована итоговое состояние транзакции неизвестно.</span><span class="sxs-lookup"><span data-stu-id="a5050-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="a5050-124">См. в этом [блога](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="a5050-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="a5050-125">По умолчанию стратегия выполнения будет повторять эту операцию, как в том случае, если откат транзакции, но если это не так это приведет к исключение, если новое состояние базы данных несовместим или может привести к **повреждения данных** Если Операция не полагаться на определенном состоянии, например, при вставке новой строки со значениями ключа, автоматически сгенерированный.</span><span class="sxs-lookup"><span data-stu-id="a5050-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="a5050-126">Существует несколько способов решения этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="a5050-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="a5050-127">Вариант 1 - выполните nothing (почти)</span><span class="sxs-lookup"><span data-stu-id="a5050-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="a5050-128">Вероятность сбоя подключения во время фиксации транзакции не хватает, поэтому он может быть приемлемым для вашего приложения с ошибкой только, если это фактически происходит.</span><span class="sxs-lookup"><span data-stu-id="a5050-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="a5050-129">Однако необходимо избегать использования сформированные хранилищем ключи для того чтобы убедиться, что исключение вместо добавления строки с повторяющимися данными.</span><span class="sxs-lookup"><span data-stu-id="a5050-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="a5050-130">Рекомендуется использовать значение идентификатора GUID, сформированное клиентом или генератор значения на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a5050-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="a5050-131">Вариант 2 - состояния приложения перестроения</span><span class="sxs-lookup"><span data-stu-id="a5050-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="a5050-132">Текущая `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a5050-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="a5050-133">Создайте новый `DbContext` и восстановить состояние приложения из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a5050-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="a5050-134">Информировать пользователей, что последняя операция может не была завершена успешно.</span><span class="sxs-lookup"><span data-stu-id="a5050-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="a5050-135">Вариант 3: добавление состояния проверки</span><span class="sxs-lookup"><span data-stu-id="a5050-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="a5050-136">Для большинства операций, которые изменяют состояние базы данных можно добавить код, который проверяет, успешно ли оно выполнено.</span><span class="sxs-lookup"><span data-stu-id="a5050-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="a5050-137">EF предоставляет метод расширения, чтобы облегчить эту задачу - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="a5050-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="a5050-138">Этот метод начинает и фиксирует транзакцию и также принимает функции в `verifySucceeded` параметр, который вызывается при возникновении временной ошибки во время фиксации транзакции.</span><span class="sxs-lookup"><span data-stu-id="a5050-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="a5050-139">Здесь `SaveChanges` вызывается с `acceptAllChangesOnSuccess` значение `false` во избежание изменения состояния `Blog` сущность `Unchanged` Если `SaveChanges` завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="a5050-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="a5050-140">Это позволит повторите ту же операцию, если фиксация завершается ошибкой и выполняется откат транзакции.</span><span class="sxs-lookup"><span data-stu-id="a5050-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="a5050-141">Вариант 4: вручную отслеживать транзакции</span><span class="sxs-lookup"><span data-stu-id="a5050-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="a5050-142">Если необходимо использовать сформированные хранилищем ключи или необходим универсальный способ обработки сбоев фиксации, не зависящей от операцию, выполняемую каждой транзакции может назначаться Идентификатором, который проверяется при сбое фиксации.</span><span class="sxs-lookup"><span data-stu-id="a5050-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="a5050-143">Добавление таблицы к базе данных, используемых для отслеживания состояния транзакций.</span><span class="sxs-lookup"><span data-stu-id="a5050-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="a5050-144">Вставьте строку в таблицу в начале каждой транзакции.</span><span class="sxs-lookup"><span data-stu-id="a5050-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="a5050-145">При сбое соединения во время фиксации, проверьте наличие соответствующей строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a5050-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="a5050-146">Если фиксация выполнена успешно, удаление соответствующей строки во избежание увеличение размера таблицы.</span><span class="sxs-lookup"><span data-stu-id="a5050-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="a5050-147">Убедитесь, что контекст, используемый для проверки стратегия выполнения задаются в качестве соединения может привести к отказу во время проверки сбоя во время фиксации транзакции.</span><span class="sxs-lookup"><span data-stu-id="a5050-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
