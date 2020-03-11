---
title: Обработка ошибок фиксации транзакций — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414677"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="2d872-102">Обработка ошибок фиксации транзакций</span><span class="sxs-lookup"><span data-stu-id="2d872-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="2d872-103">**EF 6.1 только в сторону** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="2d872-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="2d872-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="2d872-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="2d872-105">В рамках 6,1 мы представляем новую функцию обеспечения устойчивости подключений для EF: возможность обнаружения и восстановления автоматически при неустранимых сбоях подключения, влияющих на подтверждение фиксаций транзакций.</span><span class="sxs-lookup"><span data-stu-id="2d872-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="2d872-106">Полные сведения о сценарии лучше всего описаны в записи блога [Подключение к базе данных SQL и неполадки идемпотентности](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="2d872-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="2d872-107">В целом, сценарий состоит в том, что при возникновении исключения во время фиксации транзакции существует две возможные причины.</span><span class="sxs-lookup"><span data-stu-id="2d872-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="2d872-108">Сбой фиксации транзакции на сервере</span><span class="sxs-lookup"><span data-stu-id="2d872-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="2d872-109">Транзакция успешно завершена на сервере, но возникла ошибка подключения, препятствующая уведомлению об успешном завершении клиента</span><span class="sxs-lookup"><span data-stu-id="2d872-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="2d872-110">Когда в первой ситуации возникает приложение или пользователь может повторить операцию, но во втором случае следует избегать повторных попыток, и приложение может восстановиться автоматически.</span><span class="sxs-lookup"><span data-stu-id="2d872-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="2d872-111">Проблема заключается в том, что без возможности обнаруживать фактическую причину возникновения исключения во время фиксации приложение не может выбрать правильный курс действия.</span><span class="sxs-lookup"><span data-stu-id="2d872-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="2d872-112">Новая функция в EF 6,1 позволяет с помощью EF проверить базу данных, если транзакция прошла удачно, и сделать правильный курс действий прозрачным.</span><span class="sxs-lookup"><span data-stu-id="2d872-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="2d872-113">Использование компонента</span><span class="sxs-lookup"><span data-stu-id="2d872-113">Using the feature</span></span>  

<span data-ttu-id="2d872-114">Чтобы включить эту функцию, необходимо включить вызов [сеттрансактионхандлер](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) в конструкторе **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="2d872-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="2d872-115">Если вы не знакомы с **DbConfiguration**, см. раздел [Конфигурация на основе кода](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="2d872-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="2d872-116">Эта функция может использоваться в сочетании с автоматическими повторными попытками, представленными в EF6, что помогает в ситуации, когда транзакция на самом деле не зафиксирована на сервере из-за временной ошибки.</span><span class="sxs-lookup"><span data-stu-id="2d872-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="2d872-117">Как осуществляется трассировка транзакций</span><span class="sxs-lookup"><span data-stu-id="2d872-117">How transactions are tracked</span></span>  

<span data-ttu-id="2d872-118">Если эта функция включена, EF автоматически добавит новую таблицу в базу данных с именем **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="2d872-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="2d872-119">Новая строка вставляется в эту таблицу каждый раз при создании транзакции EF, и эта строка проверяется на существование, если во время фиксации происходит сбой транзакции.</span><span class="sxs-lookup"><span data-stu-id="2d872-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="2d872-120">Хотя EF будет лучше удалять строки из таблицы, когда они больше не нужны, таблица может расти, если приложение завершается преждевременно и по этой причине может потребоваться очистить таблицу вручную в некоторых случаях.</span><span class="sxs-lookup"><span data-stu-id="2d872-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="2d872-121">Как справляться со сбоями фиксации в предыдущих версиях</span><span class="sxs-lookup"><span data-stu-id="2d872-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="2d872-122">До EF 6,1 не существовал механизм на обработку ошибок фиксации в продукте EF.</span><span class="sxs-lookup"><span data-stu-id="2d872-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="2d872-123">Существует несколько способов работы с этой ситуацией, которые можно применить к предыдущим версиям EF6:</span><span class="sxs-lookup"><span data-stu-id="2d872-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="2d872-124">Вариант 1. ничего не предпринимать</span><span class="sxs-lookup"><span data-stu-id="2d872-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="2d872-125">Вероятность сбоя подключения во время фиксации транзакции мала, поэтому приложение может быть вполне неудачным, если это условие действительно происходит.</span><span class="sxs-lookup"><span data-stu-id="2d872-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="2d872-126">Вариант 2. Использование базы данных для сброса состояния</span><span class="sxs-lookup"><span data-stu-id="2d872-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="2d872-127">Отменить текущий DbContext</span><span class="sxs-lookup"><span data-stu-id="2d872-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="2d872-128">Создание нового DbContext и восстановление состояния приложения из базы данных</span><span class="sxs-lookup"><span data-stu-id="2d872-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="2d872-129">Сообщите пользователю о том, что последняя операция могла завершиться неуспешно</span><span class="sxs-lookup"><span data-stu-id="2d872-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="2d872-130">Вариант 3. Отслеживание транзакции вручную</span><span class="sxs-lookup"><span data-stu-id="2d872-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="2d872-131">Добавьте неотслеживающую таблицу в базу данных, используемую для наблюдения за состоянием транзакций.</span><span class="sxs-lookup"><span data-stu-id="2d872-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="2d872-132">Вставка строки в таблицу в начале каждой транзакции.</span><span class="sxs-lookup"><span data-stu-id="2d872-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="2d872-133">В случае сбоя соединения во время фиксации проверьте наличие соответствующей строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2d872-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="2d872-134">Если строка имеется, продолжайте обычно, так как транзакция была успешно зафиксирована.</span><span class="sxs-lookup"><span data-stu-id="2d872-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="2d872-135">Если строка отсутствует, используйте стратегию выполнения, чтобы повторить текущую операцию.</span><span class="sxs-lookup"><span data-stu-id="2d872-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="2d872-136">Если фиксация прошла успешно, удалите соответствующую строку, чтобы избежать роста таблицы.</span><span class="sxs-lookup"><span data-stu-id="2d872-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="2d872-137">[Эта запись блога](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) содержит пример кода для выполнения этой задачи на SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="2d872-137">[This blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
