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
# <a name="working-with-transactions"></a>Работа с транзакциями
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

В этом документе описывается использование транзакций в EF6, включая улучшения, добавленные с момента EF5, для упрощения работы с транзакциями.  

## <a name="what-ef-does-by-default"></a>Что делает EF по умолчанию  

Во всех версиях Entity Framework при каждом выполнении команды **SaveChanges ()** для вставки, обновления или удаления в базе данных платформа будет переносить эту операцию в транзакцию. Эта транзакция длится достаточно долго для выполнения операции, а затем завершается. При выполнении другой такой операции запускается новая транзакция.  

Начиная с версии EF6 метод **Database.ExecuteSqlCommand()** по умолчанию будет заключать команду в транзакцию, если она отсутвует. Существуют перегрузки этого метода, позволяющие при необходимости переопределять это действие. Кроме того, в EF6 выполнение хранимых процедур, включаемых в модель с помощью API-интерфейсов, таких как **ObjectContext.ExecuteFunction()**, действует таким же образом (однако переопределить это действие по умолчанию сейчас невозможно).  

В любом случае уровень изоляции транзакции — это любой уровень изоляции, который поставщик базы данных считает своим значением по умолчанию. По умолчанию, например, на SQL Server это зафиксированный READ.  

Entity Framework не переносит запросы в транзакции.  

Эта функциональность по умолчанию подходит для большого числа пользователей, и если нет необходимости делать что-либо в EF6; просто напишите код, как вы всегда делали это.  

Однако некоторым пользователям требуется больший контроль над своими транзакциями — это рассматривается в следующих разделах.  

## <a name="how-the-apis-work"></a>Как работают API  

До EF6 Entity Framework приступили к открытию подключения к базе данных (оно вызвало исключение, если было передано соединение, которое уже открыто). Так как транзакция может быть запущена только с открытым соединением, это означало, что единственным способом, которым пользователь может заключить несколько операций в одну транзакцию, является либо использование [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) , либо использование свойства **ObjectContext. Connection** и запуск вызов **Open ()** и **BeginTransaction ()** непосредственно на возвращенном объекте **EntityConnection** . Кроме того, вызовы API, которые связывались с базой данных, завершатся ошибкой, если вы запустили транзакцию в основном подключении к базе данных.  

> [!NOTE]
> Ограничение на принятие закрытых подключений было удалено в Entity Framework 6. Дополнительные сведения см. в разделе [Управление подключениями](~/ef6/fundamentals/connection-management.md).  

Начиная с EF6, платформа теперь предоставляет:  

1. **Database. BeginTransaction ()** : Более простой способ запуска и завершения транзакций в существующей DbContext — позволяет объединять несколько операций в одной и той же транзакции и, следовательно, все их фиксации или все откатываются. Он также позволяет пользователю легко указать уровень изоляции для транзакции.  
2. **Database. UseTransaction ()** : позволяет DbContext использовать транзакцию, которая была запущена за пределами Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Объединение нескольких операций в одну транзакцию в одном контексте  

**Database.BeginTransaction()** имеет два переопределения: одно принимает явное перечисление [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx), а другое не принимает аргументов и берет IsolationLevel по умолчанию от используемого поставщика базы данных. Оба переопределения возвращают объект **DbContextTransaction**, предоставляющий методы **Commit()** и **Rollback()**, которые, соответственно, выполняют фиксацию и откат для транзакции используемого хранилища.  

После фиксации или отката **DbContextTransaction** необходимо удалить. Чтобы легко сделать это, используйте синтаксис **using(…){…}** для автоматического вызова **Dispose()** при завершении using-блока: 

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
> Для начала транзакции необходимо открытое соединение с используемым хранилищем. Поэтому вызов Database.BeginTransaction() откроет соединение, если оно еще не открыто. В этом случае метод также закроет его при вызове Dispose().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Передача существующей транзакции в контекст  

Иногда требуется транзакция, которая еще более широка в области действия и включает операции с той же базой данных, но вне EF полностью. Для этого необходимо открыть подключение и начать транзакцию самостоятельно, а затем сообщить EF a) использовать уже открытое подключение к базе данных и б) для использования существующей транзакции в этом соединении.  

Для этого необходимо определить и использовать конструктор в классе контекста, который наследуется от одного из конструкторов DbContext, использующих i) существующего параметра соединения и II) логического Контекстовнсконнектион.  

> [!NOTE]
> При вызове в этом сценарии флаг contextOwnsConnection должен иметь значение false. Это важно, так как флаг информирует Entity Framework, что закрывать соединение в этом случае не нужно (например, см. строку 4 ниже): 

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Кроме того, необходимо вручную запустить транзакцию (включая IsolationLevel, если вы хотите избежать настройки по умолчанию) и разрешить Entity Framework знать о том, что в подключении уже запущена существующая транзакция (см. строку 33 ниже).  

Затем вы можете выполнять операции с базой данных непосредственно в самом объекте SqlConnection или в DbContext. Все такие операции выполняются в рамках одной транзакции. Вы несете ответственность за фиксацию или откат транзакции и вызов метода Dispose (), а также для закрытия и уничтожения подключения к базе данных. Например:  

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

### <a name="clearing-up-the-transaction"></a>Очистка транзакции

Можно передать значение NULL в Database. UseTransaction (), чтобы очистить сведения о текущей транзакции Entity Framework. Entity Framework не будет ни фиксировать, ни откатить существующую транзакцию, когда вы сделаете это, поэтому используйте с осторожностью и только в том случае, если вы уверены, что это нужно сделать.  

### <a name="errors-in-usetransaction"></a>Ошибки в UseTransaction

Вы увидите исключение из Database. UseTransaction (), если транзакция передается в следующих случаях:  
- Entity Framework уже имеет существующую транзакцию  
- Entity Framework уже работает в TransactionScope  
- Объект соединения в переданной транзакции имеет значение null. Это значит, что транзакция не связана с подключением — обычно это знак того, что транзакция уже завершена  
- Объект соединения в переданной транзакции не соответствует соединению Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Использование транзакций с другими функциями  

В этом разделе подробно описывается взаимодействие указанных выше транзакций с.  

- Устойчивость подключений  
- Асинхронные методы  
- Транзакции TransactionScope  

### <a name="connection-resiliency"></a>Устойчивость подключений  

Новая функция устойчивости соединений не работает с транзакциями, инициированными пользователем. Дополнительные сведения см. в разделе [Повторное выполнение стратегий выполнения](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Асинхронное программирование  

Для подхода, описанного в предыдущих разделах, не требуется никаких дополнительных параметров или параметров для работы с [асинхронным запросом и](~/ef6/fundamentals/async.md
)методами Save. Но имейте в виду, что в зависимости от того, что вы делает в асинхронных методах, это может привести к длительным транзакциям, что может привести к нарушению взаимоблокировок или блокировке, что неплохо для производительности всего приложения.  

### <a name="transactionscope-transactions"></a>Транзакции TransactionScope  

Прежде чем EF6 рекомендуемый способ предоставления более крупных транзакций с областью действия, следует использовать объект TransactionScope:  

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

Объекты SqlConnection и Entity Framework будут использовать внешнюю транзакцию TransactionScope и, следовательно, зафиксированы вместе.  

Начиная с .NET 4.5.1 объект TransactionScope был обновлен для работы также с асинхронными методами с помощью перечисления [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx): 

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

По-прежнему существуют некоторые ограничения для подхода TransactionScope:  

- Для работы с асинхронными методами требуется .NET 4.5.1 или более поздней версии.  
- Его нельзя использовать в облачных сценариях, если нет уверенности в наличии только одного подключения (облачные сценарии не поддерживают распределенные транзакции).  
- Его нельзя сочетать с подходом Database. UseTransaction () в предыдущих разделах.  
- Если вы выдаете DDL и не включили распределенные транзакции через службу MSDTC, выдаются исключения.  

Преимущества подхода TransactionScope:  

- Она автоматически обновит локальную транзакцию до распределенной транзакции, если вы сделаете более одного соединения с определенной базой данных или объедините соединение с одной базой данных с соединением с другой базой данных в рамках одной транзакции (Примечание. необходимо иметь Служба MSDTC, настроенная для работы с распределенными транзакциями.  
- Простота программирования. Если вы предпочитаете, чтобы транзакция была окружена и обработана неявным образом в фоновом режиме, а не при управлении, то подход TransactionScope может быть лучше.  

В целом, с помощью новых API-интерфейсов Database. BeginTransaction () и Database. UseTransaction (), описанных выше, метод TransactionScope больше не нужен большинству пользователей. Если вы продолжаете использовать TransactionScope, учитывайте указанные выше ограничения. Вместо этого рекомендуется использовать подход, описанный в предыдущих разделах.  
