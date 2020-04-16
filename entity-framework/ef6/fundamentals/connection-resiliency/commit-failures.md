---
title: Обработка сбоев транзакций - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: cf2722496e207a8ecaa9cfaa4ca61e7248e5e58f
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434140"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="0fba2-102">Обработка сбоев транзакций</span><span class="sxs-lookup"><span data-stu-id="0fba2-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="0fba2-103">**EF6.1 Только вперед** - функции, AA и т.д., обсуждаемые на этой странице, были введены в рамках entity 6.1.</span><span class="sxs-lookup"><span data-stu-id="0fba2-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="0fba2-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="0fba2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="0fba2-105">В рамках 6.1 мы вводим новую функцию устойчивости соединения для EF: возможность обнаруживать и восстанавливаться автоматически, когда переходные сбои соединения влияют на подтверждение коммитов транзакции.</span><span class="sxs-lookup"><span data-stu-id="0fba2-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="0fba2-106">Полная информация о сценарии лучше всего описана в блоге [S'L Database Connectivity и вопрос о идемпотенции](https://docs.microsoft.com/archive/blogs/adonet/sql-database-connectivity-and-the-idempotency-issue).</span><span class="sxs-lookup"><span data-stu-id="0fba2-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://docs.microsoft.com/archive/blogs/adonet/sql-database-connectivity-and-the-idempotency-issue).</span></span>  <span data-ttu-id="0fba2-107">Таким образом, сценарий заключается в том, что при поднятии исключения во время коммита транзакции существует две возможные причины:</span><span class="sxs-lookup"><span data-stu-id="0fba2-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="0fba2-108">Сбой транзакции на сервере</span><span class="sxs-lookup"><span data-stu-id="0fba2-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="0fba2-109">Транзакционный коммит удалось на сервере, но проблема подключения помешала уведомлению об успехе достичь клиента</span><span class="sxs-lookup"><span data-stu-id="0fba2-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="0fba2-110">Когда первая ситуация происходит приложение или пользователь может повторить операцию, но когда вторая ситуация происходит retries следует избегать, и приложение может восстановить автоматически.</span><span class="sxs-lookup"><span data-stu-id="0fba2-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="0fba2-111">Проблема заключается в том, что без возможности определить, что было фактической причиной исключения было сообщено во время комминажа, приложение не может выбрать правильный курс действий.</span><span class="sxs-lookup"><span data-stu-id="0fba2-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="0fba2-112">Новая функция ef 6.1 позволяет EF перепроверить с базой данных, если транзакция прошла успешно, и прозрачно принять правильный курс действий.</span><span class="sxs-lookup"><span data-stu-id="0fba2-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="0fba2-113">Использование компонента</span><span class="sxs-lookup"><span data-stu-id="0fba2-113">Using the feature</span></span>  

<span data-ttu-id="0fba2-114">Для включения функции, необходимой включить вызов [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) в конструкторе **вашего DbConfiguration.**</span><span class="sxs-lookup"><span data-stu-id="0fba2-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="0fba2-115">Если вы не знакомы с **DbConfiguration,** [см.](~/ef6/fundamentals/configuring/code-based.md)</span><span class="sxs-lookup"><span data-stu-id="0fba2-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="0fba2-116">Эта функция может быть использована в сочетании с автоматическими повторами, которые мы ввели в EF6, которые помогают в ситуации, в которой транзакция фактически не удалось совершить на сервере из-за переходного сбоя:</span><span class="sxs-lookup"><span data-stu-id="0fba2-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="0fba2-117">Как отслеживаются транзакции</span><span class="sxs-lookup"><span data-stu-id="0fba2-117">How transactions are tracked</span></span>  

<span data-ttu-id="0fba2-118">Когда функция включена, EF автоматически добавит новую таблицу в базу данных под названием **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="0fba2-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="0fba2-119">Новая строка вставляется в эту таблицу каждый раз, когда транзакция создается EF, и эта строка проверяется на наличие, если происходит сбой транзакции во время коммита.</span><span class="sxs-lookup"><span data-stu-id="0fba2-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="0fba2-120">Хотя EF прилагает все усилия, чтобы подчеркнить строки из таблицы, когда они больше не нужны, таблица может вырасти, если приложение выйдет преждевременно, и по этой причине в некоторых случаях может потребоваться очистка таблицы вручную.</span><span class="sxs-lookup"><span data-stu-id="0fba2-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="0fba2-121">Как обрабатывать сбои в сбоях с предыдущими версиями</span><span class="sxs-lookup"><span data-stu-id="0fba2-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="0fba2-122">До EF 6.1 не существовало механизма обработки сбоев в работе сбоев в продукте EF.</span><span class="sxs-lookup"><span data-stu-id="0fba2-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="0fba2-123">Существует несколько способов решения этой ситуации, которые могут быть применены к предыдущим версиям EF6:</span><span class="sxs-lookup"><span data-stu-id="0fba2-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="0fba2-124">Вариант 1 - Ничего не делать</span><span class="sxs-lookup"><span data-stu-id="0fba2-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="0fba2-125">Вероятность сбоя соединения во время совершения транзакции низка, поэтому может быть приемлемым для приложения просто сбой, если это условие действительно происходит.</span><span class="sxs-lookup"><span data-stu-id="0fba2-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="0fba2-126">Вариант 2 - Используйте базу данных для сбросить состояние</span><span class="sxs-lookup"><span data-stu-id="0fba2-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="0fba2-127">Отбросить текущий DbContext</span><span class="sxs-lookup"><span data-stu-id="0fba2-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="0fba2-128">Создание нового DbContext и восстановление состояния приложения из базы данных</span><span class="sxs-lookup"><span data-stu-id="0fba2-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="0fba2-129">Сообщите пользователю, что последняя операция, возможно, не была успешно завершена</span><span class="sxs-lookup"><span data-stu-id="0fba2-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="0fba2-130">Вариант 3 - Ручной отслеживание транзакции</span><span class="sxs-lookup"><span data-stu-id="0fba2-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="0fba2-131">Добавьте в базу данных неотслеживаемую таблицу, используемую для отслеживания состояния транзакций.</span><span class="sxs-lookup"><span data-stu-id="0fba2-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="0fba2-132">Вставьте строку в таблицу в начале каждой транзакции.</span><span class="sxs-lookup"><span data-stu-id="0fba2-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="0fba2-133">Если соединение выходит из строя во время коммина, проверьте наличие соответствующей строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0fba2-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="0fba2-134">Если строка присутствует, продолжайте нормально, так как транзакция была успешно совершена</span><span class="sxs-lookup"><span data-stu-id="0fba2-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="0fba2-135">Если строка отсутствует, используйте стратегию выполнения для повторной попытки текущей операции.</span><span class="sxs-lookup"><span data-stu-id="0fba2-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="0fba2-136">Если коммит удален, удалите соответствующий ряд, чтобы избежать роста таблицы.</span><span class="sxs-lookup"><span data-stu-id="0fba2-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="0fba2-137">[Этот блог содержит](https://docs.microsoft.com/archive/blogs/adonet/sql-database-connectivity-and-the-idempotency-issue) пример кода для выполнения этого на S'L Azure.</span><span class="sxs-lookup"><span data-stu-id="0fba2-137">[This blog post](https://docs.microsoft.com/archive/blogs/adonet/sql-database-connectivity-and-the-idempotency-issue) contains sample code for accomplishing this on SQL Azure.</span></span>  
