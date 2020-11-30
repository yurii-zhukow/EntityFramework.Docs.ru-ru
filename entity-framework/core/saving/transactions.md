---
title: Транзакции — EF Core
description: Управление транзакциями для атомарности при сохранении данных с помощью Entity Framework Core
author: roji
ms.date: 9/26/2020
uid: core/saving/transactions
ms.openlocfilehash: b5e1fa2a0bcc466f22f03fee7ecaef9dcea1efaf
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003553"
---
# <a name="using-transactions"></a><span data-ttu-id="5307f-103">Использование транзакций</span><span class="sxs-lookup"><span data-stu-id="5307f-103">Using Transactions</span></span>

<span data-ttu-id="5307f-104">Транзакции позволяют обрабатывать несколько операций с базой данных атомарным способом.</span><span class="sxs-lookup"><span data-stu-id="5307f-104">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="5307f-105">Если транзакция зафиксирована, все операции успешно применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-105">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="5307f-106">Если транзакция отменяется, ни одна из операций не применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-106">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]
> <span data-ttu-id="5307f-107">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="5307f-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="5307f-108">Поведение транзакции по умолчанию</span><span class="sxs-lookup"><span data-stu-id="5307f-108">Default transaction behavior</span></span>

<span data-ttu-id="5307f-109">По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в одном вызове `SaveChanges` применяются в транзакции.</span><span class="sxs-lookup"><span data-stu-id="5307f-109">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges` are applied in a transaction.</span></span> <span data-ttu-id="5307f-110">Если какое-либо из изменений завершается ошибкой, транзакция откатывается и ни одно из изменений не применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-110">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="5307f-111">Это означает, что операция `SaveChanges` гарантированно либо будет выполнена, либо оставит базу данных без изменений, если возникла ошибка.</span><span class="sxs-lookup"><span data-stu-id="5307f-111">This means that `SaveChanges` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="5307f-112">Для большинства приложений это поведение по умолчанию является достаточным.</span><span class="sxs-lookup"><span data-stu-id="5307f-112">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="5307f-113">Транзакциями нужно управлять вручную только в том случае, если этого требует приложение.</span><span class="sxs-lookup"><span data-stu-id="5307f-113">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="5307f-114">Управление транзакциями</span><span class="sxs-lookup"><span data-stu-id="5307f-114">Controlling transactions</span></span>

<span data-ttu-id="5307f-115">Вы можете использовать API `DbContext.Database` для начала, фиксации и отката транзакций.</span><span class="sxs-lookup"><span data-stu-id="5307f-115">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="5307f-116">В следующем примере показаны две операции `SaveChanges` и запрос LINQ, выполняемый в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="5307f-116">The following example shows two `SaveChanges` operations and a LINQ query being executed in a single transaction:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction.cs?name=Transaction&highlight=2,16-18)]

<span data-ttu-id="5307f-117">Все поставщики реляционных баз данных поддерживают транзакции. Поставщики других типов могут создавать исключения или выполнять пустые операции при вызове API транзакций.</span><span class="sxs-lookup"><span data-stu-id="5307f-117">While all relational database providers support transactions, other providers types may throw or no-op when transaction APIs are called.</span></span>

## <a name="savepoints"></a><span data-ttu-id="5307f-118">Точки сохранения</span><span class="sxs-lookup"><span data-stu-id="5307f-118">Savepoints</span></span>

> [!NOTE]
> <span data-ttu-id="5307f-119">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="5307f-119">This feature was introduced in EF Core 5.0.</span></span>

<span data-ttu-id="5307f-120">Если при вызове `SaveChanges` транзакция уже выполняется в этом контексте, EF автоматически создает *точку сохранения* перед сохранением любых данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-120">When `SaveChanges` is invoked and a transaction is already in progress on the context, EF automatically creates a *savepoint* before saving any data.</span></span> <span data-ttu-id="5307f-121">Точки сохранения — это позиции в пределах транзакции базы данных, до которых можно выполнить откат в случае возникновения ошибок.</span><span class="sxs-lookup"><span data-stu-id="5307f-121">Savepoints are points within a database transaction which may later be rolled back to, if an error occurs or for any other reason.</span></span> <span data-ttu-id="5307f-122">Если `SaveChanges` встречает любую ошибку, выполняется автоматический откат транзакции до точки сохранения, то есть транзакция возвращается в такое состояние, как если бы она не запускалась.</span><span class="sxs-lookup"><span data-stu-id="5307f-122">If `SaveChanges` encounters any error, it automatically rolls the transaction back to the savepoint, leaving the transaction in the same state as if it had never started.</span></span> <span data-ttu-id="5307f-123">Это позволяет исправить возникшие проблемы и повторно сохранить данные, что особенно важно для проблем [оптимистической блокировки](xref:core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="5307f-123">This allows you to possibly correct issues and retry saving, in particular when [optimistic concurrency](xref:core/saving/concurrency) issues occur.</span></span>

> [!WARNING]
> <span data-ttu-id="5307f-124">Точки сохранения несовместимы с множественными активными результирующими наборами SQL Server и не используются с ними.</span><span class="sxs-lookup"><span data-stu-id="5307f-124">Savepoints are incompatible with SQL Server's Multiple Active Result Sets, and are not used.</span></span> <span data-ttu-id="5307f-125">Если во время выполнения `SaveChanges` возникает ошибка, то транзакция может остаться в неизвестном состоянии.</span><span class="sxs-lookup"><span data-stu-id="5307f-125">If an error occurs during `SaveChanges`, the transaction may be left in an unknown state.</span></span>

<span data-ttu-id="5307f-126">Вы также можете вручную управлять точками сохранения, как и транзакциями.</span><span class="sxs-lookup"><span data-stu-id="5307f-126">It's also possible to manually manage savepoints, just as it is with transactions.</span></span> <span data-ttu-id="5307f-127">Следующий пример создает точку сохранения в пределах транзакции и выполняет откат к ней в случае сбоя:</span><span class="sxs-lookup"><span data-stu-id="5307f-127">The following example creates a savepoint within a transaction, and rolls back to it on failure:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ManagingSavepoints.cs?name=Savepoints&highlight=9,19-20)]

## <a name="cross-context-transaction"></a><span data-ttu-id="5307f-128">Транзакция в нескольких контекстах</span><span class="sxs-lookup"><span data-stu-id="5307f-128">Cross-context transaction</span></span>

<span data-ttu-id="5307f-129">Вы можете использовать транзакцию в нескольких экземплярах контекста.</span><span class="sxs-lookup"><span data-stu-id="5307f-129">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="5307f-130">Эта функция доступна только при использовании поставщика реляционной базы данных, так как для этого требуется использование транзакции `DbTransaction` и подключения `DbConnection`, которые относятся к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-130">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="5307f-131">Для совместного использования транзакции в контекстах должны применяться одинаковые `DbConnection` и `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="5307f-131">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="5307f-132">Разрешение подключения извне</span><span class="sxs-lookup"><span data-stu-id="5307f-132">Allow connection to be externally provided</span></span>

<span data-ttu-id="5307f-133">Для совместного использования подключения `DbConnection` требуется возможность передать подключение в контекст при его создании.</span><span class="sxs-lookup"><span data-stu-id="5307f-133">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="5307f-134">Самый простой способ разрешить предоставление `DbConnection` извне — это прекратить использование метода `DbContext.OnConfiguring`, чтобы настроить контекст и создать параметры `DbContextOptions` извне и передать их конструктору контекста.</span><span class="sxs-lookup"><span data-stu-id="5307f-134">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]
> <span data-ttu-id="5307f-135">`DbContextOptionsBuilder` — это API, который вы использовали в `DbContext.OnConfiguring` для настройки контекста. Теперь вы будете использовать его извне для создания параметров `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="5307f-135">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="5307f-136">Как вариант, можно использовать `DbContext.OnConfiguring`, но принимать подключение `DbConnection`, которое сохраняется, а затем используется в `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="5307f-136">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

```csharp
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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="5307f-137">Совместное использование подключения и транзакции</span><span class="sxs-lookup"><span data-stu-id="5307f-137">Share connection and transaction</span></span>

<span data-ttu-id="5307f-138">Теперь вы можете создать несколько экземпляров контекста, которые используют одно и то же подключение.</span><span class="sxs-lookup"><span data-stu-id="5307f-138">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="5307f-139">Затем используйте API `DbContext.Database.UseTransaction(DbTransaction)`, чтобы включить оба контекста в одну транзакцию.</span><span class="sxs-lookup"><span data-stu-id="5307f-139">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction.cs?name=Transaction&highlight=1-3,6,14,21-23)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="5307f-140">Использование внешних транзакций базы данных (только для реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="5307f-140">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="5307f-141">Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, вам может потребоваться обмениваться транзакциями между операциями, выполняемыми с помощью различных технологий.</span><span class="sxs-lookup"><span data-stu-id="5307f-141">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="5307f-142">В следующем примере показано, как выполнить операцию ADO.NET SqlClient и операцию Entity Framework Core в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="5307f-142">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction.cs?name=Transaction&highlight=4,9,20,25-27)]

## <a name="using-systemtransactions"></a><span data-ttu-id="5307f-143">Использование System.Transactions</span><span class="sxs-lookup"><span data-stu-id="5307f-143">Using System.Transactions</span></span>

<span data-ttu-id="5307f-144">Можно использовать внешние транзакции, если вам нужно координировать действия в более широком диапазоне.</span><span class="sxs-lookup"><span data-stu-id="5307f-144">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction.cs?name=Transaction&highlight=1,2,3,26-28)]

<span data-ttu-id="5307f-145">Также можно использовать явную транзакцию.</span><span class="sxs-lookup"><span data-stu-id="5307f-145">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction.cs?name=Transaction&highlight=1-2,15,28-30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="5307f-146">Ограничения System.Transactions</span><span class="sxs-lookup"><span data-stu-id="5307f-146">Limitations of System.Transactions</span></span>

1. <span data-ttu-id="5307f-147">Реализация поддержки System.Transactions в EF Core зависит от поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-147">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="5307f-148">Если поставщик не реализует поддержку System.Transactions, вызовы этих API могут полностью игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="5307f-148">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="5307f-149">SqlClient поддерживает этот вариант.</span><span class="sxs-lookup"><span data-stu-id="5307f-149">SqlClient supports it.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="5307f-150">Рекомендуется проверить, что API правильно работает с поставщиком, прежде чем использовать его для управления транзакциями.</span><span class="sxs-lookup"><span data-stu-id="5307f-150">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="5307f-151">Если он работает неправильно, рекомендуется связаться с представителем поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="5307f-151">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="5307f-152">Начиная с .NET Core 2.1, реализация System.Transactions не включает в себя поддержку распределенных транзакций, поэтому вы не можете использовать `TransactionScope` или `CommittableTransaction` для координации транзакций в нескольких диспетчерах ресурсов.</span><span class="sxs-lookup"><span data-stu-id="5307f-152">As of .NET Core 2.1, the System.Transactions implementation does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
