---
title: Транзакции — EF Core
description: Управление транзакциями для атомарности при сохранении данных с помощью Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 2674fdc59cb08e5faf2302270629c446abc2670c
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618927"
---
# <a name="using-transactions"></a><span data-ttu-id="f60c2-103">Использование транзакций</span><span class="sxs-lookup"><span data-stu-id="f60c2-103">Using Transactions</span></span>

<span data-ttu-id="f60c2-104">Транзакции позволяют обрабатывать несколько операций с базой данных атомарным способом.</span><span class="sxs-lookup"><span data-stu-id="f60c2-104">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="f60c2-105">Если транзакция зафиксирована, все операции успешно применяются к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f60c2-105">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="f60c2-106">Если транзакция отменяется, ни одна из операций не применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f60c2-106">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="f60c2-107">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="f60c2-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="f60c2-108">Поведение транзакции по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f60c2-108">Default transaction behavior</span></span>

<span data-ttu-id="f60c2-109">По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в одном вызове `SaveChanges()` применяются в транзакции.</span><span class="sxs-lookup"><span data-stu-id="f60c2-109">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="f60c2-110">Если какое-либо из изменений завершается ошибкой, транзакция откатывается и ни одно из изменений не применяется к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f60c2-110">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="f60c2-111">Это означает, что операция `SaveChanges()` гарантированно либо будет выполнена, либо оставит базу данных без изменений, если возникла ошибка.</span><span class="sxs-lookup"><span data-stu-id="f60c2-111">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="f60c2-112">Для большинства приложений это поведение по умолчанию является достаточным.</span><span class="sxs-lookup"><span data-stu-id="f60c2-112">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="f60c2-113">Транзакциями нужно управлять вручную только в том случае, если этого требует приложение.</span><span class="sxs-lookup"><span data-stu-id="f60c2-113">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="f60c2-114">Управление транзакциями</span><span class="sxs-lookup"><span data-stu-id="f60c2-114">Controlling transactions</span></span>

<span data-ttu-id="f60c2-115">Вы можете использовать API `DbContext.Database` для начала, фиксации и отката транзакций.</span><span class="sxs-lookup"><span data-stu-id="f60c2-115">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="f60c2-116">В следующем примере показаны две операции `SaveChanges()` и запрос LINQ, выполняемый в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="f60c2-116">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="f60c2-117">Не все поставщики баз данных поддерживают транзакции.</span><span class="sxs-lookup"><span data-stu-id="f60c2-117">Not all database providers support transactions.</span></span> <span data-ttu-id="f60c2-118">Некоторые поставщики могут вызывать исключение или не работать при вызове API транзакций.</span><span class="sxs-lookup"><span data-stu-id="f60c2-118">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="f60c2-119">Межконтекстная транзакция (только для реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="f60c2-119">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="f60c2-120">Вы можете использовать транзакцию в нескольких экземплярах контекста.</span><span class="sxs-lookup"><span data-stu-id="f60c2-120">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="f60c2-121">Эта функция доступна только при использовании поставщика реляционной базы данных, так как для этого требуется использование транзакции `DbTransaction` и подключения `DbConnection`, которые относятся к реляционным базам данных.</span><span class="sxs-lookup"><span data-stu-id="f60c2-121">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="f60c2-122">Для совместного использования транзакции в контекстах должны применяться одинаковые `DbConnection` и `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="f60c2-122">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="f60c2-123">Разрешение подключения извне</span><span class="sxs-lookup"><span data-stu-id="f60c2-123">Allow connection to be externally provided</span></span>

<span data-ttu-id="f60c2-124">Для совместного использования подключения `DbConnection` требуется возможность передать подключение в контекст при его создании.</span><span class="sxs-lookup"><span data-stu-id="f60c2-124">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="f60c2-125">Самый простой способ разрешить предоставление `DbConnection` извне — это прекратить использование метода `DbContext.OnConfiguring`, чтобы настроить контекст и создать параметры `DbContextOptions` извне и передать их конструктору контекста.</span><span class="sxs-lookup"><span data-stu-id="f60c2-125">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="f60c2-126">`DbContextOptionsBuilder` — это API, который вы использовали в `DbContext.OnConfiguring` для настройки контекста. Теперь вы будете использовать его извне для создания параметров `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="f60c2-126">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="f60c2-127">Как вариант, можно использовать `DbContext.OnConfiguring`, но принимать подключение `DbConnection`, которое сохраняется, а затем используется в `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="f60c2-127">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="f60c2-128">Совместное использование подключения и транзакции</span><span class="sxs-lookup"><span data-stu-id="f60c2-128">Share connection and transaction</span></span>

<span data-ttu-id="f60c2-129">Теперь вы можете создать несколько экземпляров контекста, которые используют одно и то же подключение.</span><span class="sxs-lookup"><span data-stu-id="f60c2-129">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="f60c2-130">Затем используйте API `DbContext.Database.UseTransaction(DbTransaction)`, чтобы включить оба контекста в одну транзакцию.</span><span class="sxs-lookup"><span data-stu-id="f60c2-130">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="f60c2-131">Использование внешних транзакций базы данных (только для реляционных баз данных)</span><span class="sxs-lookup"><span data-stu-id="f60c2-131">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="f60c2-132">Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, вам может потребоваться обмениваться транзакциями между операциями, выполняемыми с помощью различных технологий.</span><span class="sxs-lookup"><span data-stu-id="f60c2-132">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="f60c2-133">В следующем примере показано, как выполнить операцию ADO.NET SqlClient и операцию Entity Framework Core в одной транзакции.</span><span class="sxs-lookup"><span data-stu-id="f60c2-133">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="f60c2-134">Использование System.Transactions</span><span class="sxs-lookup"><span data-stu-id="f60c2-134">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="f60c2-135">Это новая возможность в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f60c2-135">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="f60c2-136">Можно использовать внешние транзакции, если вам нужно координировать действия в более широком диапазоне.</span><span class="sxs-lookup"><span data-stu-id="f60c2-136">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="f60c2-137">Также можно использовать явную транзакцию.</span><span class="sxs-lookup"><span data-stu-id="f60c2-137">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="f60c2-138">Ограничения System.Transactions</span><span class="sxs-lookup"><span data-stu-id="f60c2-138">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="f60c2-139">Реализация поддержки System.Transactions в EF Core зависит от поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="f60c2-139">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="f60c2-140">Хотя поддержка распространена среди поставщиков ADO.NET для .NET Framework, API только недавно был добавлен в .NET Core, поэтому на этой платформе поддержка не столь распространена.</span><span class="sxs-lookup"><span data-stu-id="f60c2-140">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="f60c2-141">Если поставщик не реализует поддержку System.Transactions, вызовы этих API могут полностью игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="f60c2-141">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="f60c2-142">SqlClient для .NET Core поддерживает эту функцию, начиная с версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="f60c2-142">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="f60c2-143">SqlClient для .NET Core 2.0 вызовет исключение, если вы попытаетесь использовать эту функцию.</span><span class="sxs-lookup"><span data-stu-id="f60c2-143">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="f60c2-144">Рекомендуется проверить, что API правильно работает с поставщиком, прежде чем использовать его для управления транзакциями.</span><span class="sxs-lookup"><span data-stu-id="f60c2-144">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="f60c2-145">Если он работает неправильно, рекомендуется связаться с представителем поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="f60c2-145">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="f60c2-146">Начиная с версии 2.1, реализация System.Transactions в .NET Core не включает в себя поддержку распределенных транзакций, поэтому вы не можете использовать `TransactionScope` или `CommittableTransaction` для координации транзакций в нескольких диспетчерах ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f60c2-146">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
