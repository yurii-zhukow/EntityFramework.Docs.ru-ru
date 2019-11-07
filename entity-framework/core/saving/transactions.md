---
title: Транзакции — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 952cb891d145a47666f1d506ec00f066be9f245d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654754"
---
# <a name="using-transactions"></a><span data-ttu-id="259c9-102">Использование транзакций</span><span class="sxs-lookup"><span data-stu-id="259c9-102">Using Transactions</span></span>

<span data-ttu-id="259c9-103">Транзакции позволяют обрабатывать несколько операций с базой данных атомарным способом.</span><span class="sxs-lookup"><span data-stu-id="259c9-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="259c9-104">Если транзакция зафиксирована, все операции успешно применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="259c9-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="259c9-105">Если транзакция отменяется, ни одна из операций не применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="259c9-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="259c9-106">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="259c9-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="259c9-107">Поведение транзакции по умолчанию</span><span class="sxs-lookup"><span data-stu-id="259c9-107">Default transaction behavior</span></span>

<span data-ttu-id="259c9-108">По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в одном вызове `SaveChanges()` применяются в транзакции.</span><span class="sxs-lookup"><span data-stu-id="259c9-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="259c9-109">Если какое-либо из изменений завершается ошибкой, транзакция откатывается и ни одно из изменений не применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="259c9-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="259c9-110">Это означает, что операция `SaveChanges()` гарантированно либо будет выполнена, либо оставит базу данных без изменений, если возникла ошибка.</span><span class="sxs-lookup"><span data-stu-id="259c9-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="259c9-111">Для большинства приложений это поведение по умолчанию является достаточным.</span><span class="sxs-lookup"><span data-stu-id="259c9-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="259c9-112">Транзакциями нужно управлять вручную только в том случае, если этого требует приложение.</span><span class="sxs-lookup"><span data-stu-id="259c9-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="259c9-113">Управление транзакциями</span><span class="sxs-lookup"><span data-stu-id="259c9-113">Controlling transactions</span></span>

<span data-ttu-id="259c9-114">Вы можете использовать API `DbContext.Database` для начала, фиксации и отката транзакций.</span><span class="sxs-lookup"><span data-stu-id="259c9-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="259c9-115">В следующем примере показаны две операции `SaveChanges()` и запрос LINQ, выполняемый в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="259c9-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="259c9-116">Не все поставщики баз данных поддерживают транзакции.</span><span class="sxs-lookup"><span data-stu-id="259c9-116">Not all database providers support transactions.</span></span> <span data-ttu-id="259c9-117">Некоторые поставщики могут вызывать исключение или не работать при вызове API транзакций.</span><span class="sxs-lookup"><span data-stu-id="259c9-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="259c9-118">Межконтекстная транзакция (только для реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="259c9-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="259c9-119">Вы можете использовать транзакцию в нескольких экземплярах контекста.</span><span class="sxs-lookup"><span data-stu-id="259c9-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="259c9-120">Эта функция доступна только при использовании поставщика реляционной базы данных, так как для этого требуется использование транзакции `DbTransaction` и подключения `DbConnection`, которые относятся к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="259c9-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="259c9-121">Для совместного использования транзакции в контекстах должны применяться одинаковые `DbConnection` и `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="259c9-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="259c9-122">Разрешение подключения извне</span><span class="sxs-lookup"><span data-stu-id="259c9-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="259c9-123">Для совместного использования подключения `DbConnection` требуется возможность передать подключение в контекст при его создании.</span><span class="sxs-lookup"><span data-stu-id="259c9-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="259c9-124">Самый простой способ разрешить предоставление `DbConnection` извне — это прекратить использование метода `DbContext.OnConfiguring`, чтобы настроить контекст и создать параметры `DbContextOptions` извне и передать их конструктору контекста.</span><span class="sxs-lookup"><span data-stu-id="259c9-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="259c9-125">`DbContextOptionsBuilder` — это API, который вы использовали в `DbContext.OnConfiguring` для настройки контекста. Теперь вы будете использовать его извне для создания параметров `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="259c9-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="259c9-126">Как вариант, можно использовать `DbContext.OnConfiguring`, но принимать подключение `DbConnection`, которое сохраняется, а затем используется в `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="259c9-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="259c9-127">Совместное использование подключения и транзакции</span><span class="sxs-lookup"><span data-stu-id="259c9-127">Share connection and transaction</span></span>

<span data-ttu-id="259c9-128">Теперь вы можете создать несколько экземпляров контекста, которые используют одно и то же подключение.</span><span class="sxs-lookup"><span data-stu-id="259c9-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="259c9-129">Затем используйте API `DbContext.Database.UseTransaction(DbTransaction)`, чтобы включить оба контекста в одну транзакцию.</span><span class="sxs-lookup"><span data-stu-id="259c9-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="259c9-130">Использование внешних транзакций базы данных (только для реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="259c9-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="259c9-131">Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, вам может потребоваться обмениваться транзакциями между операциями, выполняемыми с помощью различных технологий.</span><span class="sxs-lookup"><span data-stu-id="259c9-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="259c9-132">В следующем примере показано, как выполнить операцию ADO.NET SqlClient и операцию Entity Framework Core в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="259c9-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="259c9-133">Использование System.Transactions</span><span class="sxs-lookup"><span data-stu-id="259c9-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="259c9-134">Это новая возможность в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="259c9-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="259c9-135">Можно использовать внешние транзакции, если вам нужно координировать действия в более широком диапазоне.</span><span class="sxs-lookup"><span data-stu-id="259c9-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="259c9-136">Также можно использовать явную транзакцию.</span><span class="sxs-lookup"><span data-stu-id="259c9-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="259c9-137">Ограничения System.Transactions</span><span class="sxs-lookup"><span data-stu-id="259c9-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="259c9-138">Реализация поддержки System.Transactions в EF Core зависит от поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="259c9-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="259c9-139">Хотя поддержка распространена среди поставщиков ADO.NET для .NET Framework, API только недавно был добавлен в .NET Core, поэтому на этой платформе поддержка не столь распространена.</span><span class="sxs-lookup"><span data-stu-id="259c9-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="259c9-140">Если поставщик не реализует поддержку System.Transactions, вызовы этих API могут полностью игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="259c9-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="259c9-141">SqlClient для .NET Core поддерживает эту функцию, начиная с версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="259c9-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="259c9-142">SqlClient для .NET Core 2.0 вызовет исключение, если вы попытаетесь использовать эту функцию.</span><span class="sxs-lookup"><span data-stu-id="259c9-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="259c9-143">Рекомендуется проверить, что API правильно работает с поставщиком, прежде чем использовать его для управления транзакциями.</span><span class="sxs-lookup"><span data-stu-id="259c9-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="259c9-144">Если он работает неправильно, рекомендуется связаться с представителем поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="259c9-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="259c9-145">Начиная с версии 2.1, реализация System.Transactions в .NET Core не включает в себя поддержку распределенных транзакций, поэтому вы не можете использовать `TransactionScope` или `CommittableTransaction` для координации транзакций в нескольких диспетчерах ресурсов.</span><span class="sxs-lookup"><span data-stu-id="259c9-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
