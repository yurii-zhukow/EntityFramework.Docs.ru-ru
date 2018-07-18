---
title: Управление подключениями - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122764"
---
# <a name="connection-management"></a>Управление подключениями
Эта страница описывает поведение Entity Framework по отношению к передачи подключений в контексте и функциональные возможности **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Передача подключений к контексту  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Поведение для EF5 и более ранних версий  

Существует два конструктора, которые принимать подключения.  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Можно использовать их, но вам нужно решить ряд ограничений:  

1. При передаче открытое подключение к одному из этих затем первый раз, платформа пытается использовать его, возникает исключение InvalidOperationException о том, что не удается повторно открыть уже открытое соединение.  
2. Флаг contextOwnsConnection интерпретируется для обозначения ли Базовое соединение хранилища должен быть удален при освобождении контекста. Но, независимо от этого параметра соединения с хранилищем всегда закрывается при освобождении контекста. Поэтому при наличии нескольких DbContext того же соединения удаляется независимо от контекста сначала будет закрыть соединение (аналогично Если имеются смешанные существующее соединение ADO.NET с DbContext, DbContext будет всегда закрывать соединение после его удаления) .  

Это можно обойти первое ограничение выше путем передачи закрытого соединения и только выполнение кода, который бы открыть его, когда будут созданы все контексты:  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

Вторым ограничением просто означает, что необходимо воздержаться от удаления любой из ваших объектов DbContext, пока не будете готовы для подключения будет закрыта.  

### <a name="behavior-in-ef6-and-future-versions"></a>Поведение в EF6 и будущих версий  

В EF6 и будущих версий DbContext имеет же два конструктора, но больше не требуется, что передается конструктору подключение закрыто при его получении. Так что теперь это возможно:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

Также флаг contextOwnsConnection теперь управляет ли соединение и закрытия и удаления при освобождении DbContext. Поэтому в приведенном выше примере подключение не закрывается при контексте удален (строка 32), как это было бы в предыдущих версиях EF, но вместо этого в том случае, когда удаляется само соединение (строки 40).  

Само собой, это по-прежнему возможно для DbContext контролировать подключения (только набор contextOwnsConnection значение true, или использовать один из других конструкторов) можно.  

> [!NOTE]
> Существуют некоторые дополнительные соображения при использовании транзакций с помощью этой новой модели. Дополнительные сведения см. [работа с транзакциями](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Поведение для EF5 и более ранних версий  

В EF5 и более ранних версиях есть ошибка таким образом, чтобы **ObjectContext.Connection.State** не был обновлен для отражения состояния true базового соединения с хранилищем. Например, если выполняется следующий код вы могут возвращаться состояние **закрыто** несмотря на то, что на самом деле базового подключения к хранилищу **откройте**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Отдельно, при открытии подключения к базе данных, вызвав Database.Connection.Open() его будут открыты до следующего выполнения запроса, или вызывать никаких действий, которая требует подключения к базе данных (например, SaveChanges()), но после базового хранения, соединение будет закрыто. Контекст будет будет повторно открыть и повторно закрыть подключение каждый раз, когда необходима другая операция базы данных:  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>Поведение в EF6 и будущих версий  

Для EF6 и будущих версий мы предприняли подход, если вызывающий код выбирает для открытия соединения с вызывающим контекстом. Database.Connection.Open(), то оно имеет веские причины таким образом и framework предполагает, что он хочет контролировать Открытие и закрытие соединения и больше не закроет это подключение автоматически.  

> [!NOTE]
> Это потенциально может привести к подключениям, которые открыты для много времени, поэтому используйте с осторожностью.  

Мы также обновили код таким образом, чтобы ObjectContext.Connection.State теперь следит за состоянием базового соединения правильно.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
