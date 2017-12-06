---
title: "Операции - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a><span data-ttu-id="3e5be-102">Использование транзакций</span><span class="sxs-lookup"><span data-stu-id="3e5be-102">Using Transactions</span></span>

<span data-ttu-id="3e5be-103">Транзакции позволяют несколько операций базы данных для обработки атомарным образом.</span><span class="sxs-lookup"><span data-stu-id="3e5be-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="3e5be-104">Если транзакция будет зафиксирована, все операции успешно применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3e5be-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="3e5be-105">Если откат транзакции ни одна из операций применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3e5be-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="3e5be-106">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="3e5be-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="3e5be-107">По умолчанию поведение транзакции</span><span class="sxs-lookup"><span data-stu-id="3e5be-107">Default transaction behavior</span></span>

<span data-ttu-id="3e5be-108">По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в рамках одного вызова `SaveChanges()` применяются в транзакции.</span><span class="sxs-lookup"><span data-stu-id="3e5be-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="3e5be-109">Если изменения не проходят откат транзакции, и никакие изменения применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3e5be-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="3e5be-110">Это означает, что `SaveChanges()` гарантированно полностью завершится успехом, или оставьте без изменений, если произошла ошибка базы данных.</span><span class="sxs-lookup"><span data-stu-id="3e5be-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="3e5be-111">Это поведение по умолчанию достаточно для большинства приложений.</span><span class="sxs-lookup"><span data-stu-id="3e5be-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="3e5be-112">Транзакции должны управление вручную только в том случае, если требований приложения сочтут необходимыми.</span><span class="sxs-lookup"><span data-stu-id="3e5be-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="3e5be-113">Управление транзакциями</span><span class="sxs-lookup"><span data-stu-id="3e5be-113">Controlling transactions</span></span>

<span data-ttu-id="3e5be-114">Можно использовать `DbContext.Database` API для начала, фиксации и отката транзакций.</span><span class="sxs-lookup"><span data-stu-id="3e5be-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="3e5be-115">В следующем примере показаны два `SaveChanges()` операции и LINQ запрос, выполняемый в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="3e5be-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="3e5be-116">Не все поставщики базы данных поддерживает транзакции.</span><span class="sxs-lookup"><span data-stu-id="3e5be-116">Not all database providers support transactions.</span></span> <span data-ttu-id="3e5be-117">Некоторые поставщики могут генерировать или холостой при вызове API-интерфейсы транзакции.</span><span class="sxs-lookup"><span data-stu-id="3e5be-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="3e5be-118">Кросс контекст транзакции (только реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="3e5be-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="3e5be-119">Вы можете использовать транзакцию по нескольким экземплярам контекста.</span><span class="sxs-lookup"><span data-stu-id="3e5be-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="3e5be-120">Эта функция доступна только при использовании поставщика реляционной базы данных, так как он требует использования `DbTransaction` и `DbConnection`, которой относятся к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="3e5be-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="3e5be-121">Для совместного использования транзакции, контексты должны совместно использовать оба `DbConnection` и `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="3e5be-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="3e5be-122">Разрешить подключение должно предоставляться извне</span><span class="sxs-lookup"><span data-stu-id="3e5be-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="3e5be-123">Совместное использование `DbConnection` требуется возможность передавать соединение в контекст, при его создании.</span><span class="sxs-lookup"><span data-stu-id="3e5be-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="3e5be-124">Самый простой способ разрешить `DbConnection` извне, в том, — чтобы прекратить использование `DbContext.OnConfiguring` метод, чтобы настроить контекст и извне создать `DbContextOptions` и передавать их в контексте конструктора.</span><span class="sxs-lookup"><span data-stu-id="3e5be-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="3e5be-125">`DbContextOptionsBuilder`API-интерфейс, который использовался в `DbContext.OnConfiguring` Чтобы настроить контекст, теперь необходимо использовать его для создания внешнего `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="3e5be-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

<span data-ttu-id="3e5be-126">Альтернативным вариантом является для продолжения использования `DbContext.OnConfiguring`, но принимает `DbConnection` , сохраняется, а затем использовать в `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="3e5be-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a><span data-ttu-id="3e5be-127">Общий ресурс соединения и транзакции</span><span class="sxs-lookup"><span data-stu-id="3e5be-127">Share connection and transaction</span></span>

<span data-ttu-id="3e5be-128">Теперь можно создать несколько экземпляров контекст, в которых одно соединение.</span><span class="sxs-lookup"><span data-stu-id="3e5be-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="3e5be-129">Затем с помощью `DbContext.Database.UseTransaction(DbTransaction)` API для прикрепления обоих контекстов в той же транзакции.</span><span class="sxs-lookup"><span data-stu-id="3e5be-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="3e5be-130">С помощью внешних DbTransactions (только реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="3e5be-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="3e5be-131">Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, можно совместно использовать транзакции между операции, выполняемые этими различных технологий.</span><span class="sxs-lookup"><span data-stu-id="3e5be-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="3e5be-132">Приведенный ниже показано, как выполнить операцию ADO.NET SqlClient Entity Framework Core операции в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="3e5be-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
