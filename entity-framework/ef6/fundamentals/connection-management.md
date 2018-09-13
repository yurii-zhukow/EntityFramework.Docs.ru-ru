---
title: Управление подключениями - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489340"
---
# <a name="connection-management"></a><span data-ttu-id="57457-102">Управление подключениями</span><span class="sxs-lookup"><span data-stu-id="57457-102">Connection management</span></span>
<span data-ttu-id="57457-103">Эта страница описывает поведение Entity Framework по отношению к передачи подключений в контексте и функциональные возможности **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="57457-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="57457-104">Передача подключений к контексту</span><span class="sxs-lookup"><span data-stu-id="57457-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="57457-105">Поведение для EF5 и более ранних версий</span><span class="sxs-lookup"><span data-stu-id="57457-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="57457-106">Существует два конструктора, которые принимать подключения.</span><span class="sxs-lookup"><span data-stu-id="57457-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="57457-107">Можно использовать их, но вам нужно решить ряд ограничений:</span><span class="sxs-lookup"><span data-stu-id="57457-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="57457-108">При передаче открытое подключение к одному из этих затем первый раз, платформа пытается использовать его, возникает исключение InvalidOperationException о том, что не удается повторно открыть уже открытое соединение.</span><span class="sxs-lookup"><span data-stu-id="57457-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="57457-109">Флаг contextOwnsConnection интерпретируется для обозначения ли Базовое соединение хранилища должен быть удален при освобождении контекста.</span><span class="sxs-lookup"><span data-stu-id="57457-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="57457-110">Но, независимо от этого параметра соединения с хранилищем всегда закрывается при освобождении контекста.</span><span class="sxs-lookup"><span data-stu-id="57457-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="57457-111">Поэтому при наличии нескольких DbContext того же соединения удаляется независимо от контекста сначала будет закрыть соединение (аналогично Если имеются смешанные существующее соединение ADO.NET с DbContext, DbContext будет всегда закрывать соединение после его удаления) .</span><span class="sxs-lookup"><span data-stu-id="57457-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="57457-112">Это можно обойти первое ограничение выше путем передачи закрытого соединения и только выполнение кода, который бы открыть его, когда будут созданы все контексты:</span><span class="sxs-lookup"><span data-stu-id="57457-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="57457-113">Вторым ограничением просто означает, что необходимо воздержаться от удаления любой из ваших объектов DbContext, пока не будете готовы для подключения будет закрыта.</span><span class="sxs-lookup"><span data-stu-id="57457-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="57457-114">Поведение в EF6 и будущих версий</span><span class="sxs-lookup"><span data-stu-id="57457-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="57457-115">В EF6 и будущих версий DbContext имеет же два конструктора, но больше не требуется, что передается конструктору подключение закрыто при его получении.</span><span class="sxs-lookup"><span data-stu-id="57457-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="57457-116">Так что теперь это возможно:</span><span class="sxs-lookup"><span data-stu-id="57457-116">So this is now possible:</span></span>  

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

<span data-ttu-id="57457-117">Также флаг contextOwnsConnection теперь управляет ли соединение и закрытия и удаления при освобождении DbContext.</span><span class="sxs-lookup"><span data-stu-id="57457-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="57457-118">Поэтому в приведенном выше примере подключение не закрывается при контексте удален (строка 32), как это было бы в предыдущих версиях EF, но вместо этого в том случае, когда удаляется само соединение (строки 40).</span><span class="sxs-lookup"><span data-stu-id="57457-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="57457-119">Само собой, это по-прежнему возможно для DbContext контролировать подключения (только набор contextOwnsConnection значение true, или использовать один из других конструкторов) можно.</span><span class="sxs-lookup"><span data-stu-id="57457-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="57457-120">Существуют некоторые дополнительные соображения при использовании транзакций с помощью этой новой модели.</span><span class="sxs-lookup"><span data-stu-id="57457-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="57457-121">Дополнительные сведения см. [работа с транзакциями](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="57457-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="57457-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="57457-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="57457-123">Поведение для EF5 и более ранних версий</span><span class="sxs-lookup"><span data-stu-id="57457-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="57457-124">В EF5 и более ранних версиях есть ошибка таким образом, чтобы **ObjectContext.Connection.State** не был обновлен для отражения состояния true базового соединения с хранилищем.</span><span class="sxs-lookup"><span data-stu-id="57457-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="57457-125">Например, если выполняется следующий код вы могут возвращаться состояние **закрыто** несмотря на то, что на самом деле базового подключения к хранилищу **откройте**.</span><span class="sxs-lookup"><span data-stu-id="57457-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="57457-126">Отдельно, при открытии подключения к базе данных, вызвав Database.Connection.Open() его будут открыты до следующего выполнения запроса, или вызывать никаких действий, которая требует подключения к базе данных (например, SaveChanges()), но после базового хранения, соединение будет закрыто.</span><span class="sxs-lookup"><span data-stu-id="57457-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="57457-127">Контекст будет будет повторно открыть и повторно закрыть подключение каждый раз, когда необходима другая операция базы данных:</span><span class="sxs-lookup"><span data-stu-id="57457-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="57457-128">Поведение в EF6 и будущих версий</span><span class="sxs-lookup"><span data-stu-id="57457-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="57457-129">Для EF6 и будущих версий мы предприняли подход, если вызывающий код выбирает для открытия соединения с вызывающим контекстом. Database.Connection.Open(), то оно имеет веские причины таким образом и framework предполагает, что он хочет контролировать Открытие и закрытие соединения и больше не закроет это подключение автоматически.</span><span class="sxs-lookup"><span data-stu-id="57457-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="57457-130">Это потенциально может привести к подключениям, которые открыты для много времени, поэтому используйте с осторожностью.</span><span class="sxs-lookup"><span data-stu-id="57457-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="57457-131">Мы также обновили код таким образом, чтобы ObjectContext.Connection.State теперь следит за состоянием базового соединения правильно.</span><span class="sxs-lookup"><span data-stu-id="57457-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
