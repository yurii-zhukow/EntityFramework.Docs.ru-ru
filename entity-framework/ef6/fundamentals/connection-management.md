---
title: Управление подключениями — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414869"
---
# <a name="connection-management"></a>Управление соединениями
На этой странице описывается поведение Entity Framework с точки зрения передачи подключений к контексту и функциональных возможностей API **базы данных. Connection. Open ()** .  

## <a name="passing-connections-to-the-context"></a>Передача подключений в контекст  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Поведение для EF5 и более ранних версий  

Существует два конструктора, которые принимают подключения:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Их можно использовать, но необходимо обойти несколько ограничений:  

1. Если передать открытое соединение одному из них, то при первой попытке платформы использовать исключение InvalidOperationException возникает, что он не может повторно открыть уже открытое соединение.  
2. Флаг Контекстовнсконнектион интерпретируется как значение, указывающее, следует ли удалять соединение базового хранилища при удалении контекста. Однако, независимо от этого параметра, подключение к хранилищу всегда закрывается при удалении контекста. Поэтому при наличии нескольких DbContext с одним и тем же подключением при удалении контекста сначала будет закрыто подключение (аналогично, если существующее подключение ADO.NET к DbContext, DbContext всегда закроет подключение при его удалении). .  

Можно обойти первое ограничение выше, передав закрытое соединение и только выполнив код, который будет открывать его после создания всех контекстов:  

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

Второе ограничение просто означает, что необходимо отменять удаление любых объектов DbContext, пока вы не будете готовы к закрытию соединения.  

### <a name="behavior-in-ef6-and-future-versions"></a>Поведение в EF6 и будущих версиях  

В EF6 и будущих версиях DbContext имеет те же два конструктора, но больше не требует, чтобы соединение, передаваемое конструктору, было закрыто при получении. Теперь это возможно:  

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

Кроме того, флаг Контекстовнсконнектион определяет, будет ли соединение закрыто и удалено при удалении DbContext. Поэтому в приведенном выше примере соединение не закрывается при удалении контекста (строка 32), как было бы в предыдущих версиях EF, а когда соединение удаляется (строка 40).  

Конечно, DbContext может получить контроль над подключением (просто установите Контекстовнсконнектион в true или используйте один из других конструкторов), если это необходимо.  

> [!NOTE]
> При использовании транзакций с этой новой моделью необходимо учитывать некоторые дополнительные моменты. Дополнительные сведения см. [в разделе Работа с транзакциями](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>База данных. Connection. Open ()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Поведение для EF5 и более ранних версий  

В EF5 и более ранних версиях существует ошибка, в результате которой **ObjectContext. Connection. State** не обновлялся, чтобы отразить истинное состояние базового соединения с хранилищем. Например, если вы выполнили следующий код, вы можете вернуть состояние **закрыто** , хотя на самом деле **открыто**подключение к базовому хранилищу.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Отдельно, если подключение к базе данных открывается путем вызова базы данных. Connection. Open (), оно будет открыто до следующего выполнения запроса или вызова любого, для которого требуется подключение к базе данных (например, SaveChanges ()), но после этого хранилища соединение будет закрыто. Затем контекст повторно откроет подключение и закроет его повторно, когда потребуется другая операция базы данных:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>Поведение в EF6 и будущих версиях  

Для EF6 и будущих версий мы воспользовались подходом, при котором вызывающий код выбирает открытие соединения путем вызова context. База данных. Connection. Open (), она имеет хорошую причину, и платформа предполагает, что она хочет контролировать открытие и закрытие подключения и больше не будет автоматически закрывать подключение.  

> [!NOTE]
> Это потенциально может привести к тому, что подключения, открытые в течение длительного времени, будут использоваться с осторожностью.  

Мы также обновили код так, чтобы ObjectContext. Connection. штат теперь правильно отслеживает состояние базового соединения.  

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
