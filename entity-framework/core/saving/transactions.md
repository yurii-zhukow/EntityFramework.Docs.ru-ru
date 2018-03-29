---
title: Операции - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: fe4c0d6ad7ccb2e97dc94fbf2eb26a41e7fbcb19
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="7c60e-102">Использование транзакций</span><span class="sxs-lookup"><span data-stu-id="7c60e-102">Using Transactions</span></span>

<span data-ttu-id="7c60e-103">Транзакции позволяют несколько операций базы данных для обработки атомарным образом.</span><span class="sxs-lookup"><span data-stu-id="7c60e-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="7c60e-104">Если транзакция будет зафиксирована, все операции успешно применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="7c60e-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="7c60e-105">Если откат транзакции ни одна из операций применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="7c60e-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="7c60e-106">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="7c60e-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="7c60e-107">По умолчанию поведение транзакции</span><span class="sxs-lookup"><span data-stu-id="7c60e-107">Default transaction behavior</span></span>

<span data-ttu-id="7c60e-108">По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в рамках одного вызова `SaveChanges()` применяются в транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="7c60e-109">Если изменения не проходят откат транзакции, и никакие изменения применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="7c60e-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="7c60e-110">Это означает, что `SaveChanges()` гарантированно полностью завершится успехом, или оставьте без изменений, если произошла ошибка базы данных.</span><span class="sxs-lookup"><span data-stu-id="7c60e-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="7c60e-111">Это поведение по умолчанию достаточно для большинства приложений.</span><span class="sxs-lookup"><span data-stu-id="7c60e-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="7c60e-112">Транзакции должны управление вручную только в том случае, если требований приложения сочтут необходимыми.</span><span class="sxs-lookup"><span data-stu-id="7c60e-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="7c60e-113">Управление транзакциями</span><span class="sxs-lookup"><span data-stu-id="7c60e-113">Controlling transactions</span></span>

<span data-ttu-id="7c60e-114">Можно использовать `DbContext.Database` API для начала, фиксации и отката транзакций.</span><span class="sxs-lookup"><span data-stu-id="7c60e-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="7c60e-115">В следующем примере показаны два `SaveChanges()` операции и LINQ запрос, выполняемый в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="7c60e-116">Не все поставщики базы данных поддерживает транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-116">Not all database providers support transactions.</span></span> <span data-ttu-id="7c60e-117">Некоторые поставщики могут генерировать или холостой при вызове API-интерфейсы транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="7c60e-118">Кросс контекст транзакции (только реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="7c60e-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="7c60e-119">Вы можете использовать транзакцию по нескольким экземплярам контекста.</span><span class="sxs-lookup"><span data-stu-id="7c60e-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="7c60e-120">Эта функция доступна только при использовании поставщика реляционной базы данных, так как он требует использования `DbTransaction` и `DbConnection`, которой относятся к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="7c60e-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="7c60e-121">Для совместного использования транзакции, контексты должны совместно использовать оба `DbConnection` и `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="7c60e-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="7c60e-122">Разрешить подключение должно предоставляться извне</span><span class="sxs-lookup"><span data-stu-id="7c60e-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="7c60e-123">Совместное использование `DbConnection` требуется возможность передавать соединение в контекст, при его создании.</span><span class="sxs-lookup"><span data-stu-id="7c60e-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="7c60e-124">Самый простой способ разрешить `DbConnection` извне, в том, — чтобы прекратить использование `DbContext.OnConfiguring` метод, чтобы настроить контекст и извне создать `DbContextOptions` и передавать их в контексте конструктора.</span><span class="sxs-lookup"><span data-stu-id="7c60e-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="7c60e-125">`DbContextOptionsBuilder` API-интерфейс, который использовался в `DbContext.OnConfiguring` Чтобы настроить контекст, теперь необходимо использовать его для создания внешнего `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="7c60e-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="7c60e-126">Альтернативным вариантом является для продолжения использования `DbContext.OnConfiguring`, но принимает `DbConnection` , сохраняется, а затем использовать в `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="7c60e-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="7c60e-127">Общий ресурс соединения и транзакции</span><span class="sxs-lookup"><span data-stu-id="7c60e-127">Share connection and transaction</span></span>

<span data-ttu-id="7c60e-128">Теперь можно создать несколько экземпляров контекст, в которых одно соединение.</span><span class="sxs-lookup"><span data-stu-id="7c60e-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="7c60e-129">Затем с помощью `DbContext.Database.UseTransaction(DbTransaction)` API для прикрепления обоих контекстов в той же транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="7c60e-130">С помощью внешних DbTransactions (только реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="7c60e-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="7c60e-131">Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, можно совместно использовать транзакции между операции, выполняемые этими различных технологий.</span><span class="sxs-lookup"><span data-stu-id="7c60e-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="7c60e-132">Приведенный ниже показано, как выполнить операцию ADO.NET SqlClient Entity Framework Core операции в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="7c60e-133">Использование System.Transactions</span><span class="sxs-lookup"><span data-stu-id="7c60e-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="7c60e-134">Этот компонент впервые появился в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7c60e-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="7c60e-135">Можно использовать внешней транзакции, если необходимо управлять с большей областью действия.</span><span class="sxs-lookup"><span data-stu-id="7c60e-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="7c60e-136">Можно также прикрепить в явной транзакции.</span><span class="sxs-lookup"><span data-stu-id="7c60e-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="7c60e-137">Ограничения System.Transactions</span><span class="sxs-lookup"><span data-stu-id="7c60e-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="7c60e-138">EF Core зависит от поставщиков базы данных, чтобы обеспечить поддержку System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="7c60e-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="7c60e-139">Несмотря на то, что поддержка довольно часто поставщиков ADO.NET для .NET Framework, API-Интерфейс только недавно добавлено .NET Core и поэтому поддержки не является быть настолько распространена.</span><span class="sxs-lookup"><span data-stu-id="7c60e-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="7c60e-140">Если поставщик не реализует поддержку System.Transactions, можно будет полностью обрабатывается вызовы этих интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="7c60e-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="7c60e-141">SqlClient для .NET Core, она поддерживается с версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="7c60e-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="7c60e-142">SqlClient 2.0 .NET Core будет вызывать исключение из попробуйте использовать функцию.</span><span class="sxs-lookup"><span data-stu-id="7c60e-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="7c60e-143">Рекомендуется проверить, что API правильно работает с поставщиком перед полагаться на него для управления транзакциями.</span><span class="sxs-lookup"><span data-stu-id="7c60e-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="7c60e-144">Вы, рекомендуется связаться программы обслуживания поставщика базы данных, если это не так.</span><span class="sxs-lookup"><span data-stu-id="7c60e-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="7c60e-145">Начиная с версии 2.1, реализация System.Transactions в .NET Core отсутствует поддержка распределенных транзакций, поэтому нельзя использовать `TransactionScope` или `CommitableTransaction` для координации транзакций через несколько диспетчеров ресурсов.</span><span class="sxs-lookup"><span data-stu-id="7c60e-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
