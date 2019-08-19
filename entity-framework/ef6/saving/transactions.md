---
title: Работа с транзакциями — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306526"
---
# <a name="working-with-transactions"></a><span data-ttu-id="12889-102">Работа с транзакциями</span><span class="sxs-lookup"><span data-stu-id="12889-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="12889-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="12889-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="12889-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="12889-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="12889-105">В этом документе описывается использование транзакций в EF6, включая улучшения, добавленные с момента EF5, для упрощения работы с транзакциями.</span><span class="sxs-lookup"><span data-stu-id="12889-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="12889-106">Что делает EF по умолчанию</span><span class="sxs-lookup"><span data-stu-id="12889-106">What EF does by default</span></span>  

<span data-ttu-id="12889-107">Во всех версиях Entity Framework при каждом выполнении команды **SaveChanges ()** для вставки, обновления или удаления в базе данных платформа будет переносить эту операцию в транзакцию.</span><span class="sxs-lookup"><span data-stu-id="12889-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="12889-108">Эта транзакция длится достаточно долго для выполнения операции, а затем завершается.</span><span class="sxs-lookup"><span data-stu-id="12889-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="12889-109">При выполнении другой такой операции запускается новая транзакция.</span><span class="sxs-lookup"><span data-stu-id="12889-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="12889-110">Начиная с версии EF6 метод **Database.ExecuteSqlCommand()** по умолчанию будет заключать команду в транзакцию, если она отсутвует.</span><span class="sxs-lookup"><span data-stu-id="12889-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="12889-111">Существуют перегрузки этого метода, позволяющие при необходимости переопределять это действие.</span><span class="sxs-lookup"><span data-stu-id="12889-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="12889-112">Кроме того, в EF6 выполнение хранимых процедур, включаемых в модель с помощью API-интерфейсов, таких как **ObjectContext.ExecuteFunction()** , действует таким же образом (однако переопределить это действие по умолчанию сейчас невозможно).</span><span class="sxs-lookup"><span data-stu-id="12889-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="12889-113">В любом случае уровень изоляции транзакции — это любой уровень изоляции, который поставщик базы данных считает своим значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="12889-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="12889-114">По умолчанию, например, на SQL Server это зафиксированный READ.</span><span class="sxs-lookup"><span data-stu-id="12889-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="12889-115">Entity Framework не переносит запросы в транзакции.</span><span class="sxs-lookup"><span data-stu-id="12889-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="12889-116">Эта функциональность по умолчанию подходит для большого числа пользователей, и если нет необходимости делать что-либо в EF6; просто напишите код, как вы всегда делали это.</span><span class="sxs-lookup"><span data-stu-id="12889-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="12889-117">Однако некоторым пользователям требуется больший контроль над своими транзакциями — это рассматривается в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="12889-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="12889-118">Как работают API</span><span class="sxs-lookup"><span data-stu-id="12889-118">How the APIs work</span></span>  

<span data-ttu-id="12889-119">До EF6 Entity Framework приступили к открытию подключения к базе данных (оно вызвало исключение, если было передано соединение, которое уже открыто).</span><span class="sxs-lookup"><span data-stu-id="12889-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="12889-120">Так как транзакция может быть запущена только с открытым соединением, это означало, что единственным способом, которым пользователь может заключить несколько операций в одну транзакцию, является либо использование [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) , либо использование свойства **ObjectContext. Connection** и запуск вызов **Open ()** и **BeginTransaction ()** непосредственно на возвращенном объекте **EntityConnection** .</span><span class="sxs-lookup"><span data-stu-id="12889-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="12889-121">Кроме того, вызовы API, которые связывались с базой данных, завершатся ошибкой, если вы запустили транзакцию в основном подключении к базе данных.</span><span class="sxs-lookup"><span data-stu-id="12889-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="12889-122">Ограничение на принятие закрытых подключений было удалено в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="12889-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="12889-123">Дополнительные сведения см. в разделе [Управление подключениями](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="12889-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="12889-124">Начиная с EF6, платформа теперь предоставляет:</span><span class="sxs-lookup"><span data-stu-id="12889-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="12889-125">**Database. BeginTransaction ()** : Более простой способ запуска и завершения транзакций в существующей DbContext — позволяет объединять несколько операций в одной и той же транзакции и, следовательно, все их фиксации или все откатываются.</span><span class="sxs-lookup"><span data-stu-id="12889-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="12889-126">Он также позволяет пользователю легко указать уровень изоляции для транзакции.</span><span class="sxs-lookup"><span data-stu-id="12889-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="12889-127">**Database. UseTransaction ()** : позволяет DbContext использовать транзакцию, которая была запущена за пределами Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="12889-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="12889-128">Объединение нескольких операций в одну транзакцию в одном контексте</span><span class="sxs-lookup"><span data-stu-id="12889-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="12889-129">**Database.BeginTransaction()** имеет два переопределения: одно принимает явное перечисление [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx), а другое не принимает аргументов и берет IsolationLevel по умолчанию от используемого поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="12889-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="12889-130">Оба переопределения возвращают объект **DbContextTransaction**, предоставляющий методы **Commit()** и **Rollback()** , которые, соответственно, выполняют фиксацию и откат для транзакции используемого хранилища.</span><span class="sxs-lookup"><span data-stu-id="12889-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="12889-131">После фиксации или отката **DbContextTransaction** необходимо удалить.</span><span class="sxs-lookup"><span data-stu-id="12889-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="12889-132">Чтобы легко сделать это, используйте синтаксис **using(…){…}**</span><span class="sxs-lookup"><span data-stu-id="12889-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="12889-133">для автоматического вызова **Dispose()** при завершении using-блока:</span><span class="sxs-lookup"><span data-stu-id="12889-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    context.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'"
                        );

                    var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }

                    context.SaveChanges();

                    dbContextTransaction.Commit();
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="12889-134">Для начала транзакции необходимо открытое соединение с используемым хранилищем.</span><span class="sxs-lookup"><span data-stu-id="12889-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="12889-135">Поэтому вызов Database.BeginTransaction() откроет соединение, если оно еще не открыто.</span><span class="sxs-lookup"><span data-stu-id="12889-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="12889-136">В этом случае метод также закроет его при вызове Dispose().</span><span class="sxs-lookup"><span data-stu-id="12889-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="12889-137">Передача существующей транзакции в контекст</span><span class="sxs-lookup"><span data-stu-id="12889-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="12889-138">Иногда требуется транзакция, которая еще более широка в области действия и включает операции с той же базой данных, но вне EF полностью.</span><span class="sxs-lookup"><span data-stu-id="12889-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="12889-139">Для этого необходимо открыть подключение и начать транзакцию самостоятельно, а затем сообщить EF a) использовать уже открытое подключение к базе данных и б) для использования существующей транзакции в этом соединении.</span><span class="sxs-lookup"><span data-stu-id="12889-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="12889-140">Для этого необходимо определить и использовать конструктор в классе контекста, который наследуется от одного из конструкторов DbContext, использующих i) существующего параметра соединения и II) логического Контекстовнсконнектион.</span><span class="sxs-lookup"><span data-stu-id="12889-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="12889-141">При вызове в этом сценарии для флага Контекстовнсконнектион должно быть задано значение false.</span><span class="sxs-lookup"><span data-stu-id="12889-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="12889-142">Это важно, так как флаг информирует Entity Framework, что закрывать соединение в этом случае не нужно (например, см. строку 4 ниже):</span><span class="sxs-lookup"><span data-stu-id="12889-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="12889-143">Кроме того, необходимо вручную запустить транзакцию (включая IsolationLevel, если вы хотите избежать настройки по умолчанию) и разрешить Entity Framework знать о том, что в подключении уже запущена существующая транзакция (см. строку 33 ниже).</span><span class="sxs-lookup"><span data-stu-id="12889-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="12889-144">Затем вы можете выполнять операции с базой данных непосредственно в самом объекте SqlConnection или в DbContext.</span><span class="sxs-lookup"><span data-stu-id="12889-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="12889-145">Все такие операции выполняются в рамках одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="12889-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="12889-146">Вы несете ответственность за фиксацию или откат транзакции и вызов метода Dispose (), а также для закрытия и уничтожения подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="12889-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="12889-147">Например:</span><span class="sxs-lookup"><span data-stu-id="12889-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   var sqlCommand = new SqlCommand();
                   sqlCommand.Connection = conn;
                   sqlCommand.Transaction = sqlTxn;
                   sqlCommand.CommandText =
                       @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'";
                   sqlCommand.ExecuteNonQuery();

                   using (var context =  
                     new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        context.Database.UseTransaction(sqlTxn);

                        var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                       context.SaveChanges();
                    }

                    sqlTxn.Commit();
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="12889-148">Очистка транзакции</span><span class="sxs-lookup"><span data-stu-id="12889-148">Clearing up the transaction</span></span>

<span data-ttu-id="12889-149">Можно передать значение NULL в Database. UseTransaction (), чтобы очистить сведения о текущей транзакции Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="12889-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="12889-150">Entity Framework не будет ни фиксировать, ни откатить существующую транзакцию, когда вы сделаете это, поэтому используйте с осторожностью и только в том случае, если вы уверены, что это нужно сделать.</span><span class="sxs-lookup"><span data-stu-id="12889-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="12889-151">Ошибки в UseTransaction</span><span class="sxs-lookup"><span data-stu-id="12889-151">Errors in UseTransaction</span></span>

<span data-ttu-id="12889-152">Вы увидите исключение из Database. UseTransaction (), если транзакция передается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="12889-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="12889-153">Entity Framework уже имеет существующую транзакцию</span><span class="sxs-lookup"><span data-stu-id="12889-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="12889-154">Entity Framework уже работает в TransactionScope</span><span class="sxs-lookup"><span data-stu-id="12889-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="12889-155">Объект соединения в переданной транзакции имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="12889-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="12889-156">Это значит, что транзакция не связана с подключением — обычно это знак того, что транзакция уже завершена</span><span class="sxs-lookup"><span data-stu-id="12889-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="12889-157">Объект соединения в переданной транзакции не соответствует соединению Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="12889-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="12889-158">Использование транзакций с другими функциями</span><span class="sxs-lookup"><span data-stu-id="12889-158">Using transactions with other features</span></span>  

<span data-ttu-id="12889-159">В этом разделе подробно описывается взаимодействие указанных выше транзакций с.</span><span class="sxs-lookup"><span data-stu-id="12889-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="12889-160">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="12889-160">Connection resiliency</span></span>  
- <span data-ttu-id="12889-161">Асинхронные методы</span><span class="sxs-lookup"><span data-stu-id="12889-161">Asynchronous methods</span></span>  
- <span data-ttu-id="12889-162">Транзакции TransactionScope</span><span class="sxs-lookup"><span data-stu-id="12889-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="12889-163">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="12889-163">Connection Resiliency</span></span>  

<span data-ttu-id="12889-164">Новая функция устойчивости соединений не работает с транзакциями, инициированными пользователем.</span><span class="sxs-lookup"><span data-stu-id="12889-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="12889-165">Дополнительные сведения см. в разделе [Повторное выполнение стратегий выполнения](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="12889-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="12889-166">Асинхронное программирование</span><span class="sxs-lookup"><span data-stu-id="12889-166">Asynchronous Programming</span></span>  

<span data-ttu-id="12889-167">Для подхода, описанного в предыдущих разделах, не требуется никаких дополнительных параметров или параметров для работы с [асинхронным запросом и](~/ef6/fundamentals/async.md
)методами Save.</span><span class="sxs-lookup"><span data-stu-id="12889-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="12889-168">Но имейте в виду, что в зависимости от того, что вы делает в асинхронных методах, это может привести к длительным транзакциям, что может привести к нарушению взаимоблокировок или блокировке, что неплохо для производительности всего приложения.</span><span class="sxs-lookup"><span data-stu-id="12889-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="12889-169">Транзакции TransactionScope</span><span class="sxs-lookup"><span data-stu-id="12889-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="12889-170">Прежде чем EF6 рекомендуемый способ предоставления более крупных транзакций с областью действия, следует использовать объект TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="12889-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="12889-171">Объекты SqlConnection и Entity Framework будут использовать внешнюю транзакцию TransactionScope и, следовательно, зафиксированы вместе.</span><span class="sxs-lookup"><span data-stu-id="12889-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="12889-172">Начиная с .NET 4.5.1 объект TransactionScope был обновлен для работы также с асинхронными методами с помощью перечисления [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx):</span><span class="sxs-lookup"><span data-stu-id="12889-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="12889-173">По-прежнему существуют некоторые ограничения для подхода TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="12889-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="12889-174">Для работы с асинхронными методами требуется .NET 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="12889-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="12889-175">Его нельзя использовать в облачных сценариях, если нет уверенности в наличии только одного подключения (облачные сценарии не поддерживают распределенные транзакции).</span><span class="sxs-lookup"><span data-stu-id="12889-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="12889-176">Его нельзя сочетать с подходом Database. UseTransaction () в предыдущих разделах.</span><span class="sxs-lookup"><span data-stu-id="12889-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="12889-177">Если вы выдаете DDL и не включили распределенные транзакции через службу MSDTC, выдаются исключения.</span><span class="sxs-lookup"><span data-stu-id="12889-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="12889-178">Преимущества подхода TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="12889-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="12889-179">Она автоматически обновит локальную транзакцию до распределенной транзакции, если вы сделаете более одного соединения с определенной базой данных или объедините соединение с одной базой данных с соединением с другой базой данных в рамках одной транзакции (Примечание. необходимо иметь Служба MSDTC, настроенная для работы с распределенными транзакциями.</span><span class="sxs-lookup"><span data-stu-id="12889-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="12889-180">Простота программирования.</span><span class="sxs-lookup"><span data-stu-id="12889-180">Ease of coding.</span></span> <span data-ttu-id="12889-181">Если вы предпочитаете, чтобы транзакция была окружена и обработана неявным образом в фоновом режиме, а не при управлении, то подход TransactionScope может быть лучше.</span><span class="sxs-lookup"><span data-stu-id="12889-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="12889-182">В целом, с помощью новых API-интерфейсов Database. BeginTransaction () и Database. UseTransaction (), описанных выше, метод TransactionScope больше не нужен большинству пользователей.</span><span class="sxs-lookup"><span data-stu-id="12889-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="12889-183">Если вы продолжаете использовать TransactionScope, учитывайте указанные выше ограничения.</span><span class="sxs-lookup"><span data-stu-id="12889-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="12889-184">Вместо этого рекомендуется использовать подход, описанный в предыдущих разделах.</span><span class="sxs-lookup"><span data-stu-id="12889-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
