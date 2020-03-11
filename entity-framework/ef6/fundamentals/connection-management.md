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
# <a name="connection-management"></a><span data-ttu-id="4c60e-102">Управление соединениями</span><span class="sxs-lookup"><span data-stu-id="4c60e-102">Connection management</span></span>
<span data-ttu-id="4c60e-103">На этой странице описывается поведение Entity Framework с точки зрения передачи подключений к контексту и функциональных возможностей API **базы данных. Connection. Open ()** .</span><span class="sxs-lookup"><span data-stu-id="4c60e-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="4c60e-104">Передача подключений в контекст</span><span class="sxs-lookup"><span data-stu-id="4c60e-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="4c60e-105">Поведение для EF5 и более ранних версий</span><span class="sxs-lookup"><span data-stu-id="4c60e-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="4c60e-106">Существует два конструктора, которые принимают подключения:</span><span class="sxs-lookup"><span data-stu-id="4c60e-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="4c60e-107">Их можно использовать, но необходимо обойти несколько ограничений:</span><span class="sxs-lookup"><span data-stu-id="4c60e-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="4c60e-108">Если передать открытое соединение одному из них, то при первой попытке платформы использовать исключение InvalidOperationException возникает, что он не может повторно открыть уже открытое соединение.</span><span class="sxs-lookup"><span data-stu-id="4c60e-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="4c60e-109">Флаг Контекстовнсконнектион интерпретируется как значение, указывающее, следует ли удалять соединение базового хранилища при удалении контекста.</span><span class="sxs-lookup"><span data-stu-id="4c60e-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="4c60e-110">Однако, независимо от этого параметра, подключение к хранилищу всегда закрывается при удалении контекста.</span><span class="sxs-lookup"><span data-stu-id="4c60e-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="4c60e-111">Поэтому при наличии нескольких DbContext с одним и тем же подключением при удалении контекста сначала будет закрыто подключение (аналогично, если существующее подключение ADO.NET к DbContext, DbContext всегда закроет подключение при его удалении). .</span><span class="sxs-lookup"><span data-stu-id="4c60e-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="4c60e-112">Можно обойти первое ограничение выше, передав закрытое соединение и только выполнив код, который будет открывать его после создания всех контекстов:</span><span class="sxs-lookup"><span data-stu-id="4c60e-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="4c60e-113">Второе ограничение просто означает, что необходимо отменять удаление любых объектов DbContext, пока вы не будете готовы к закрытию соединения.</span><span class="sxs-lookup"><span data-stu-id="4c60e-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="4c60e-114">Поведение в EF6 и будущих версиях</span><span class="sxs-lookup"><span data-stu-id="4c60e-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="4c60e-115">В EF6 и будущих версиях DbContext имеет те же два конструктора, но больше не требует, чтобы соединение, передаваемое конструктору, было закрыто при получении.</span><span class="sxs-lookup"><span data-stu-id="4c60e-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="4c60e-116">Теперь это возможно:</span><span class="sxs-lookup"><span data-stu-id="4c60e-116">So this is now possible:</span></span>  

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

<span data-ttu-id="4c60e-117">Кроме того, флаг Контекстовнсконнектион определяет, будет ли соединение закрыто и удалено при удалении DbContext.</span><span class="sxs-lookup"><span data-stu-id="4c60e-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="4c60e-118">Поэтому в приведенном выше примере соединение не закрывается при удалении контекста (строка 32), как было бы в предыдущих версиях EF, а когда соединение удаляется (строка 40).</span><span class="sxs-lookup"><span data-stu-id="4c60e-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="4c60e-119">Конечно, DbContext может получить контроль над подключением (просто установите Контекстовнсконнектион в true или используйте один из других конструкторов), если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="4c60e-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="4c60e-120">При использовании транзакций с этой новой моделью необходимо учитывать некоторые дополнительные моменты.</span><span class="sxs-lookup"><span data-stu-id="4c60e-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="4c60e-121">Дополнительные сведения см. [в разделе Работа с транзакциями](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="4c60e-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="4c60e-122">База данных. Connection. Open ()</span><span class="sxs-lookup"><span data-stu-id="4c60e-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="4c60e-123">Поведение для EF5 и более ранних версий</span><span class="sxs-lookup"><span data-stu-id="4c60e-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="4c60e-124">В EF5 и более ранних версиях существует ошибка, в результате которой **ObjectContext. Connection. State** не обновлялся, чтобы отразить истинное состояние базового соединения с хранилищем.</span><span class="sxs-lookup"><span data-stu-id="4c60e-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="4c60e-125">Например, если вы выполнили следующий код, вы можете вернуть состояние **закрыто** , хотя на самом деле **открыто**подключение к базовому хранилищу.</span><span class="sxs-lookup"><span data-stu-id="4c60e-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="4c60e-126">Отдельно, если подключение к базе данных открывается путем вызова базы данных. Connection. Open (), оно будет открыто до следующего выполнения запроса или вызова любого, для которого требуется подключение к базе данных (например, SaveChanges ()), но после этого хранилища соединение будет закрыто.</span><span class="sxs-lookup"><span data-stu-id="4c60e-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="4c60e-127">Затем контекст повторно откроет подключение и закроет его повторно, когда потребуется другая операция базы данных:</span><span class="sxs-lookup"><span data-stu-id="4c60e-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="4c60e-128">Поведение в EF6 и будущих версиях</span><span class="sxs-lookup"><span data-stu-id="4c60e-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="4c60e-129">Для EF6 и будущих версий мы воспользовались подходом, при котором вызывающий код выбирает открытие соединения путем вызова context. База данных. Connection. Open (), она имеет хорошую причину, и платформа предполагает, что она хочет контролировать открытие и закрытие подключения и больше не будет автоматически закрывать подключение.</span><span class="sxs-lookup"><span data-stu-id="4c60e-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="4c60e-130">Это потенциально может привести к тому, что подключения, открытые в течение длительного времени, будут использоваться с осторожностью.</span><span class="sxs-lookup"><span data-stu-id="4c60e-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="4c60e-131">Мы также обновили код так, чтобы ObjectContext. Connection. штат теперь правильно отслеживает состояние базового соединения.</span><span class="sxs-lookup"><span data-stu-id="4c60e-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
