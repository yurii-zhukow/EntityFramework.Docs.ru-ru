---
title: Работа с транзакциями - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 96cfff4cca59ab27dd68f50d0260e90902e33a92
ms.sourcegitcommit: eefcab31142f61a7aaeac03ea90dcd39f158b8b8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873238"
---
# <a name="working-with-transactions"></a>Работа с транзакциями
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

В этом документе описано использование транзакций в EF6, включая усовершенствования, реализованной с момента EF5 для облегчения работы с транзакциями.  

## <a name="what-ef-does-by-default"></a>По умолчанию не EF  

Во всех версиях Entity Framework, при каждом выполнении **SaveChanges()** вставить, обновить или удалить на базе платформы будет служить оболочкой этой операции в транзакции. Данная транзакция продолжается только достаточно долго для того, для выполнения операции, а затем завершает работу. При выполнении другой операции запускается новая транзакция.  

Начиная с EF6 **Database.ExecuteSqlCommand()** по умолчанию будет служить оболочкой команды в транзакции, если один еще не. Существуют перегрузки этого метода, которые дают возможность переопределить это поведение, при необходимости. Также в EF6 выполнение хранимых процедур, включенных в модель через API-интерфейсы, такие как **ObjectContext.ExecuteFunction()** делает то же самое (за исключением того, что поведение по умолчанию не может в данный момент переопределяться).  

В любом случае уровень изоляции транзакции — независимо от уровня изоляции считает, что поставщик базы данных по умолчанию этого параметра. По умолчанию например, на сервере SQL Server это READ COMMITTED.  

Платформа Entity Framework не перенос запросов в транзакции.  

Эта функция по умолчанию подходит для многих пользователей, и если так что нет необходимости делать какие-либо изменения в EF6; просто напишите код, как это делалось всегда.  

Однако некоторым пользователям требуется больший контроль над их транзакций — это рассматривается в следующих разделах.  

## <a name="how-the-apis-work"></a>Работе с API  

Прежде чем платформа Entity Framework EF6 настаивали на том, на открытие подключения к базе данных сам (он вызывал исключение, если был передан уже было открыто соединение). Так как транзакции можно запустить только на открытое соединение, это означало, что единственным способом поместить несколько операций в одну транзакцию пользователя было использовать [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) или использовать  **ObjectContext.Connection** и начала вызова **Open()** и **BeginTransaction()** непосредственно в возвращенном **EntityConnection** объект. Кроме того вызовы API, которые подключиться к базе данных завершается ошибкой, если транзакция была запущена на самостоятельно базового подключения к базе данных.  

> [!NOTE]
> Ограничение принятие только закрытых подключений был удален в Entity Framework 6. Дополнительные сведения см. в разделе [Управление соединениями](~/ef6/fundamentals/connection-management.md).  

Начиная с EF6 framework теперь предоставляет:  

1. **Database.BeginTransaction()** : Более простой метод для пользователя для запуска и завершения транзакций, сами в существующих DbContext — позволяя выполнять несколько операций для объединения в той же транзакции и, следовательно все зафиксированные или все как один откат. Он также позволяет пользователю легче указать уровень изоляции для транзакции.  
2. **Database.UseTransaction()** : что позволяет использовать транзакцию, в которой был запущен вне платформы Entity Framework DbContext.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Объединение нескольких операций в одну транзакцию, в том же контексте  

**Database.BeginTransaction()** позволяет переопределять – один, принимающий явно [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) и которая не принимает аргументы и использует значение по умолчанию IsolationLevel от базового поставщика базы данных. Оба переопределения возвращать **DbContextTransaction** объект, который предоставляет **Commit()** и **Rollback()** методы, выполняющие commit и rollback в базовом хранилище транзакция.  

**DbContextTransaction** должен быть удален после фиксации или отката. Ищете простой способ выполнения этой задачи является **using(...) {...}** синтаксис, который автоматически вызовет **Dispose()** при блока using завершения:  

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
> Запуск транзакции требуется Базовое соединение хранилища открытый. Поэтому вызов Database.BeginTransaction() откроет соединение, если он еще не открыт. Если соединение открыто DbContextTransaction затем будут закрыты его при вызове Dispose().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Передача существующей транзакции контекста  

Иногда требуется транзакция становится еще шире, в области, а также полностью включает операции на той же базе данных, но вне EF. Для выполнения этой задачи необходимо открыть подключение и начать транзакцию, самостоятельно и затем сообщить EF) для использования подключения к базе данных уже открыт и (б) для использования существующей транзакции для этого соединения.  

Для этого необходимо определить и использовать конструктор в классе контекста, который наследуется от одного или конструкторов DbContext, которые принимают логическое i) существующего параметра подключения и contextOwnsConnection ii.  

> [!NOTE]
> Флаг contextOwnsConnection должно быть присвоено значение false при вызове в этом сценарии. Это важно, так как она сообщает об Entity Framework, она не должна закрывать соединение когда он закончила с ним (например, см. в разделе строки 4 ниже):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Более того, необходимо начать транзакцию, самостоятельно (включая IsolationLevel, если вы хотите избежать значение по умолчанию) и сообщить Entity Framework узнать о существующей транзакции уже запущен на соединение (см. в разделе строки 33 ниже).  

Затем вы можете свободно для выполнения операций с базой данных, либо непосредственно на сам SqlConnection, либо на DbContext. Все эти операции выполняются в рамках одной транзакции. Вы отвечаете для фиксации или отката транзакции и для вызова Dispose() на нем, а также для закрытия и удаления подключения к базе данных. Пример:  

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

### <a name="clearing-up-the-transaction"></a>Очистка в операции

Можно передать null Database.UseTransaction() снимите знаний Entity Framework текущей транзакции. Платформа Entity Framework будет ни фиксация, ни откат существующей транзакции, при этом, поэтому следует использовать с осторожностью и только в том случае, если вы точно знаете, это будет выполняться.  

### <a name="errors-in-usetransaction"></a>Ошибки в UseTransaction

Вы увидите исключения из Database.UseTransaction() при передаче транзакции при:  
- Платформа Entity Framework уже существующей транзакции  
- Платформа Entity Framework уже работает в класс TransactionScope  
- Объект соединения в транзакции, передан имеет значение null. То есть транзакция не связан с соединением – обычно это означает, что транзакция уже завершена  
- Объект соединения в транзакции, передан не соответствует подключения Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Использование транзакций с другими компонентами  

В этом разделе описаны как выше транзакции взаимодействуют с:  

- Устойчивость подключений  
- Асинхронные методы  
- Операции TransactionScope  

### <a name="connection-resiliency"></a>Устойчивость подключений  

Новая функция устойчивости подключений не работает с транзакциями, инициированного пользователем. Дополнительные сведения см. в разделе [стратегий выполнения повторов](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Асинхронное программирование  

Подход, описанный в предыдущих разделах требуется не Дополнительные параметры или параметры для работы с [асинхронного запроса и сохранения методов](~/ef6/fundamentals/async.md
). Но имейте в виду, что, в зависимости от того, что можно делать в асинхронные методы, это может привести длительные транзакции – что в свою очередь может привести к взаимоблокировкам или блокировки, который является негативно сказаться на производительности приложения в целом.  

### <a name="transactionscope-transactions"></a>Операции TransactionScope  

До EF6 рекомендованный способ предоставления большего размера область транзакции был использовать объект TransactionScope:  

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

SqlConnection и Entity Framework будет использовать внешнюю транзакцию TransactionScope и поэтому выполняться одновременная фиксация.  

Начиная с .NET 4.5.1 TransactionScope был обновлен для работы с асинхронных методов с помощью [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) перечисления:  

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

По-прежнему имеют определенные ограничения TransactionScope подход:  

- Требуется .NET 4.5.1 или более поздней версии для работы с асинхронными методами.  
- Он не может использоваться в облачных сценариях кроме случаев, когда у вас есть только одно подключение (облачные сценарии не поддерживают распределенные транзакции).  
- Можно сочетать с подходом Database.UseTransaction() из предыдущих разделов.  
- Он будет вызывать исключения, если выдавать DDL и не включили распределенные транзакции через службу MSDTC.  

Преимущества подхода TransactionScope:  

- Он автоматически обновит локальной транзакции до распределенной транзакции, если сделать несколько подключений к заданной базе данных или объединить подключения к одной базе данных с подключением к другой базе данных в той же транзакции (Примечание: необходимо иметь Служба MSDTC разрешать распределенные транзакции для правильной работы).  
- Простота написания кода. Если вы предпочитаете транзакции окружающей среды и раздачи с неявно в фоновом режиме, а не явным образом в элемент управления затем TransactionScope подход может вам подходят лучше.  

Таким образом, с помощью новых Database.BeginTransaction() и выше, API-интерфейсы Database.UseTransaction() подход TransactionScope больше не требуется для большинства пользователей. Если вы продолжите использовать TransactionScope затем следует учитывать ограничения выше. Мы рекомендуем использовать подход, описанный в предыдущих разделах, вместо этого, где это возможно.  
