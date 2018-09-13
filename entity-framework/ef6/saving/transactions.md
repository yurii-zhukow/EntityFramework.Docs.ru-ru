---
title: Работа с транзакциями - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7197733ab25c8475746e7863963384730919e3ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489782"
---
# <a name="working-with-transactions"></a><span data-ttu-id="44446-102">Работа с транзакциями</span><span class="sxs-lookup"><span data-stu-id="44446-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="44446-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="44446-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="44446-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="44446-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="44446-105">В этом документе описано использование транзакций в EF6, включая усовершенствования, реализованной с момента EF5 для облегчения работы с транзакциями.</span><span class="sxs-lookup"><span data-stu-id="44446-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="44446-106">По умолчанию не EF</span><span class="sxs-lookup"><span data-stu-id="44446-106">What EF does by default</span></span>  

<span data-ttu-id="44446-107">Во всех версиях Entity Framework, при каждом выполнении **SaveChanges()** вставить, обновить или удалить на базе платформы будет служить оболочкой этой операции в транзакции.</span><span class="sxs-lookup"><span data-stu-id="44446-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="44446-108">Данная транзакция продолжается только достаточно долго для того, для выполнения операции, а затем завершает работу.</span><span class="sxs-lookup"><span data-stu-id="44446-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="44446-109">При выполнении другой операции запускается новая транзакция.</span><span class="sxs-lookup"><span data-stu-id="44446-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="44446-110">Начиная с EF6 **Database.ExecuteSqlCommand()** по умолчанию будет служить оболочкой команды в транзакции, если один еще не.</span><span class="sxs-lookup"><span data-stu-id="44446-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="44446-111">Существуют перегрузки этого метода, которые дают возможность переопределить это поведение, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="44446-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="44446-112">Также в EF6 выполнение хранимых процедур, включенных в модель через API-интерфейсы, такие как **ObjectContext.ExecuteFunction()** делает то же самое (за исключением того, что поведение по умолчанию не может в данный момент переопределяться).</span><span class="sxs-lookup"><span data-stu-id="44446-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="44446-113">В любом случае уровень изоляции транзакции — независимо от уровня изоляции считает, что поставщик базы данных по умолчанию этого параметра.</span><span class="sxs-lookup"><span data-stu-id="44446-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="44446-114">По умолчанию например, на сервере SQL Server это READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="44446-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="44446-115">Платформа Entity Framework не перенос запросов в транзакции.</span><span class="sxs-lookup"><span data-stu-id="44446-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="44446-116">Эта функция по умолчанию подходит для многих пользователей, и если так что нет необходимости делать какие-либо изменения в EF6; просто напишите код, как это делалось всегда.</span><span class="sxs-lookup"><span data-stu-id="44446-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="44446-117">Однако некоторым пользователям требуется больший контроль над их транзакций — это рассматривается в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="44446-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="44446-118">Работе с API</span><span class="sxs-lookup"><span data-stu-id="44446-118">How the APIs work</span></span>  

<span data-ttu-id="44446-119">Прежде чем платформа Entity Framework EF6 настаивали на том, на открытие подключения к базе данных сам (он вызывал исключение, если был передан уже было открыто соединение).</span><span class="sxs-lookup"><span data-stu-id="44446-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="44446-120">Так как транзакции можно запустить только на открытое соединение, это означало, что единственным способом поместить несколько операций в одну транзакцию пользователя было использовать [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) или использовать  **ObjectContext.Connection** и начала вызова **Open()** и **BeginTransaction()** непосредственно в возвращенном **EntityConnection** объект.</span><span class="sxs-lookup"><span data-stu-id="44446-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="44446-121">Кроме того вызовы API, которые подключиться к базе данных завершается ошибкой, если транзакция была запущена на самостоятельно базового подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="44446-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="44446-122">Ограничение принятие только закрытых подключений был удален в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="44446-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="44446-123">Дополнительные сведения см. в разделе [Управление соединениями](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="44446-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="44446-124">Начиная с EF6 framework теперь предоставляет:</span><span class="sxs-lookup"><span data-stu-id="44446-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="44446-125">**Database.BeginTransaction()** : более простой метод для пользователя для запуска и завершения транзакций, сами в существующих DbContext — позволяя выполнять несколько операций для объединения в той же транзакции и, следовательно все зафиксированные или все откат как один.</span><span class="sxs-lookup"><span data-stu-id="44446-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="44446-126">Он также позволяет пользователю легче указать уровень изоляции для транзакции.</span><span class="sxs-lookup"><span data-stu-id="44446-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="44446-127">**Database.UseTransaction()** : что позволяет использовать транзакцию, в которой был запущен вне платформы Entity Framework DbContext.</span><span class="sxs-lookup"><span data-stu-id="44446-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="44446-128">Объединение нескольких операций в одну транзакцию, в том же контексте</span><span class="sxs-lookup"><span data-stu-id="44446-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="44446-129">**Database.BeginTransaction()** позволяет переопределять – один, принимающий явно [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) и которая не принимает аргументы и использует значение по умолчанию IsolationLevel от базового поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="44446-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="44446-130">Оба переопределения возвращать **DbContextTransaction** объект, который предоставляет **Commit()** и **Rollback()** методы, выполняющие commit и rollback в базовом хранилище транзакция.</span><span class="sxs-lookup"><span data-stu-id="44446-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="44446-131">**DbContextTransaction** должен быть удален после фиксации или отката.</span><span class="sxs-lookup"><span data-stu-id="44446-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="44446-132">Ищете простой способ выполнения этой задачи является **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="44446-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="44446-133">синтаксис, который автоматически вызовет **Dispose()** при блока using завершения:</span><span class="sxs-lookup"><span data-stu-id="44446-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
                    try
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
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="44446-134">Запуск транзакции требуется Базовое соединение хранилища открытый.</span><span class="sxs-lookup"><span data-stu-id="44446-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="44446-135">Поэтому вызов Database.BeginTransaction() откроет соединение, если он еще не открыт.</span><span class="sxs-lookup"><span data-stu-id="44446-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="44446-136">Если соединение открыто DbContextTransaction затем будут закрыты его при вызове Dispose().</span><span class="sxs-lookup"><span data-stu-id="44446-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="44446-137">Передача существующей транзакции контекста</span><span class="sxs-lookup"><span data-stu-id="44446-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="44446-138">Иногда требуется транзакция становится еще шире, в области, а также полностью включает операции на той же базе данных, но вне EF.</span><span class="sxs-lookup"><span data-stu-id="44446-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="44446-139">Для выполнения этой задачи необходимо открыть подключение и начать транзакцию, самостоятельно и затем сообщить EF) для использования подключения к базе данных уже открыт и (б) для использования существующей транзакции для этого соединения.</span><span class="sxs-lookup"><span data-stu-id="44446-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="44446-140">Для этого необходимо определить и использовать конструктор в классе контекста, который наследуется от одного или конструкторов DbContext, которые принимают логическое i) существующего параметра подключения и contextOwnsConnection ii.</span><span class="sxs-lookup"><span data-stu-id="44446-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="44446-141">Флаг contextOwnsConnection должно быть присвоено значение false при вызове в этом сценарии.</span><span class="sxs-lookup"><span data-stu-id="44446-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="44446-142">Это важно, так как она сообщает об Entity Framework, она не должна закрывать соединение когда он закончила с ним (например, см. в разделе строки 4 ниже):</span><span class="sxs-lookup"><span data-stu-id="44446-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="44446-143">Более того, необходимо начать транзакцию, самостоятельно (включая IsolationLevel, если вы хотите избежать значение по умолчанию) и сообщить Entity Framework узнать о существующей транзакции уже запущен на соединение (см. в разделе строки 33 ниже).</span><span class="sxs-lookup"><span data-stu-id="44446-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="44446-144">Затем вы можете свободно для выполнения операций с базой данных, либо непосредственно на сам SqlConnection, либо на DbContext.</span><span class="sxs-lookup"><span data-stu-id="44446-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="44446-145">Все эти операции выполняются в рамках одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="44446-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="44446-146">Вы отвечаете для фиксации или отката транзакции и для вызова Dispose() на нем, а также для закрытия и удаления подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="44446-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="44446-147">Пример:</span><span class="sxs-lookup"><span data-stu-id="44446-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

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
                   try
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
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="44446-148">Очистка в операции</span><span class="sxs-lookup"><span data-stu-id="44446-148">Clearing up the transaction</span></span>

<span data-ttu-id="44446-149">Можно передать null Database.UseTransaction() снимите знаний Entity Framework текущей транзакции.</span><span class="sxs-lookup"><span data-stu-id="44446-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="44446-150">Платформа Entity Framework будет ни фиксация, ни откат существующей транзакции, при этом, поэтому следует использовать с осторожностью и только в том случае, если вы точно знаете, это будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="44446-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="44446-151">Ошибки в UseTransaction</span><span class="sxs-lookup"><span data-stu-id="44446-151">Errors in UseTransaction</span></span>

<span data-ttu-id="44446-152">Вы увидите исключения из Database.UseTransaction() при передаче транзакции при:</span><span class="sxs-lookup"><span data-stu-id="44446-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="44446-153">Платформа Entity Framework уже существующей транзакции</span><span class="sxs-lookup"><span data-stu-id="44446-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="44446-154">Платформа Entity Framework уже работает в класс TransactionScope</span><span class="sxs-lookup"><span data-stu-id="44446-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="44446-155">Объект соединения в транзакции, передан имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="44446-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="44446-156">То есть транзакция не связан с соединением – обычно это означает, что транзакция уже завершена</span><span class="sxs-lookup"><span data-stu-id="44446-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="44446-157">Объект соединения в транзакции, передан не соответствует подключения Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="44446-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="44446-158">Использование транзакций с другими компонентами</span><span class="sxs-lookup"><span data-stu-id="44446-158">Using transactions with other features</span></span>  

<span data-ttu-id="44446-159">В этом разделе описаны как выше транзакции взаимодействуют с:</span><span class="sxs-lookup"><span data-stu-id="44446-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="44446-160">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="44446-160">Connection resiliency</span></span>  
- <span data-ttu-id="44446-161">Асинхронные методы</span><span class="sxs-lookup"><span data-stu-id="44446-161">Asynchronous methods</span></span>  
- <span data-ttu-id="44446-162">Операции TransactionScope</span><span class="sxs-lookup"><span data-stu-id="44446-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="44446-163">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="44446-163">Connection Resiliency</span></span>  

<span data-ttu-id="44446-164">Новая функция устойчивости подключений не работает с транзакциями, инициированного пользователем.</span><span class="sxs-lookup"><span data-stu-id="44446-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="44446-165">Дополнительные сведения см. в разделе [стратегий выполнения повторов](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="44446-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="44446-166">Асинхронное программирование</span><span class="sxs-lookup"><span data-stu-id="44446-166">Asynchronous Programming</span></span>  

<span data-ttu-id="44446-167">Подход, описанный в предыдущих разделах требуется не Дополнительные параметры или параметры для работы с [асинхронного запроса и сохранения методов](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="44446-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="44446-168">Но имейте в виду, что, в зависимости от того, что можно делать в асинхронные методы, это может привести длительные транзакции – что в свою очередь может привести к взаимоблокировкам или блокировки, который является негативно сказаться на производительности приложения в целом.</span><span class="sxs-lookup"><span data-stu-id="44446-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="44446-169">Операции TransactionScope</span><span class="sxs-lookup"><span data-stu-id="44446-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="44446-170">До EF6 рекомендованный способ предоставления большего размера область транзакции был использовать объект TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="44446-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="44446-171">SqlConnection и Entity Framework будет использовать внешнюю транзакцию TransactionScope и поэтому выполняться одновременная фиксация.</span><span class="sxs-lookup"><span data-stu-id="44446-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="44446-172">Начиная с .NET 4.5.1 TransactionScope был обновлен для работы с асинхронных методов с помощью [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) перечисления:</span><span class="sxs-lookup"><span data-stu-id="44446-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="44446-173">По-прежнему имеют определенные ограничения TransactionScope подход:</span><span class="sxs-lookup"><span data-stu-id="44446-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="44446-174">Требуется .NET 4.5.1 или более поздней версии для работы с асинхронными методами.</span><span class="sxs-lookup"><span data-stu-id="44446-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="44446-175">Он не может использоваться в облачных сценариях кроме случаев, когда у вас есть только одно подключение (облачные сценарии не поддерживают распределенные транзакции).</span><span class="sxs-lookup"><span data-stu-id="44446-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="44446-176">Можно сочетать с подходом Database.UseTransaction() из предыдущих разделов.</span><span class="sxs-lookup"><span data-stu-id="44446-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="44446-177">Он будет вызывать исключения, если выдавать DDL и не включили распределенные транзакции через службу MSDTC.</span><span class="sxs-lookup"><span data-stu-id="44446-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="44446-178">Преимущества подхода TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="44446-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="44446-179">Он автоматически обновит локальной транзакции до распределенной транзакции, если сделать несколько подключений к заданной базе данных или объединить подключения к одной базе данных с подключением к другой базе данных в той же транзакции (Примечание: необходимо иметь Служба MSDTC разрешать распределенные транзакции для правильной работы).</span><span class="sxs-lookup"><span data-stu-id="44446-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="44446-180">Простота написания кода.</span><span class="sxs-lookup"><span data-stu-id="44446-180">Ease of coding.</span></span> <span data-ttu-id="44446-181">Если вы предпочитаете транзакции окружающей среды и раздачи с неявно в фоновом режиме, а не явным образом в элемент управления затем TransactionScope подход может вам подходят лучше.</span><span class="sxs-lookup"><span data-stu-id="44446-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="44446-182">Таким образом, с помощью новых Database.BeginTransaction() и выше, API-интерфейсы Database.UseTransaction() подход TransactionScope больше не требуется для большинства пользователей.</span><span class="sxs-lookup"><span data-stu-id="44446-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="44446-183">Если вы продолжите использовать TransactionScope затем следует учитывать ограничения выше.</span><span class="sxs-lookup"><span data-stu-id="44446-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="44446-184">Мы рекомендуем использовать подход, описанный в предыдущих разделах, вместо этого, где это возможно.</span><span class="sxs-lookup"><span data-stu-id="44446-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
