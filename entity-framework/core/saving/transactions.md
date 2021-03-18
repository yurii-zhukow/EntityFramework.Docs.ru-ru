---
title: Транзакции — EF Core
description: Управление транзакциями для атомарности при сохранении данных с помощью Entity Framework Core
author: roji
ms.date: 9/26/2020
uid: core/saving/transactions
ms.openlocfilehash: 25bf615cffa36384de9fe04fe2b84d4a6bca98d0
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023540"
---
# <a name="using-transactions"></a>Использование транзакций

Транзакции позволяют обрабатывать несколько операций с базой данных атомарным способом. Если транзакция зафиксирована, все операции успешно применяются к базе данных. Если транзакция отменяется, ни одна из операций не применяется к базе данных.

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Saving/Transactions/) из репозитория GitHub.

## <a name="default-transaction-behavior"></a>Поведение транзакции по умолчанию

По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в одном вызове `SaveChanges` применяются в транзакции. Если какое-либо из изменений завершается ошибкой, транзакция откатывается и ни одно из изменений не применяется к базе данных. Это означает, что операция `SaveChanges` гарантированно либо будет выполнена, либо оставит базу данных без изменений, если возникла ошибка.

Для большинства приложений это поведение по умолчанию является достаточным. Транзакциями нужно управлять вручную только в том случае, если этого требует приложение.

## <a name="controlling-transactions"></a>Управление транзакциями

Вы можете использовать API `DbContext.Database` для начала, фиксации и отката транзакций. В следующем примере показаны две операции `SaveChanges` и запрос LINQ, выполняемый в одной транзакции.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction.cs?name=Transaction&highlight=2,16-18)]

Все поставщики реляционных баз данных поддерживают транзакции. Поставщики других типов могут создавать исключения или выполнять пустые операции при вызове API транзакций.

## <a name="savepoints"></a>Точки сохранения

> [!NOTE]
> Эта возможность появилась в EF Core 5.0.

Если при вызове `SaveChanges` транзакция уже выполняется в этом контексте, EF автоматически создает *точку сохранения* перед сохранением любых данных. Точки сохранения — это позиции в пределах транзакции базы данных, до которых можно выполнить откат в случае возникновения ошибок. Если `SaveChanges` встречает любую ошибку, выполняется автоматический откат транзакции до точки сохранения, то есть транзакция возвращается в такое состояние, как если бы она не запускалась. Это позволяет исправить возникшие проблемы и повторно сохранить данные, что особенно важно для проблем [оптимистической блокировки](xref:core/saving/concurrency).

> [!WARNING]
> Точки сохранения несовместимы с множественными активными результирующими наборами SQL Server и не используются с ними. Если во время выполнения `SaveChanges` возникает ошибка, то транзакция может остаться в неизвестном состоянии.

Вы также можете вручную управлять точками сохранения, как и транзакциями. Следующий пример создает точку сохранения в пределах транзакции и выполняет откат к ней в случае сбоя:

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ManagingSavepoints.cs?name=Savepoints&highlight=9,19-20)]

## <a name="cross-context-transaction"></a>Транзакция в нескольких контекстах

Вы можете использовать транзакцию в нескольких экземплярах контекста. Эта функция доступна только при использовании поставщика реляционной базы данных, так как для этого требуется использование транзакции `DbTransaction` и подключения `DbConnection`, которые относятся к реляционным базам данных.

Для совместного использования транзакции в контекстах должны применяться одинаковые `DbConnection` и `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Разрешение подключения извне

Для совместного использования подключения `DbConnection` требуется возможность передать подключение в контекст при его создании.

Самый простой способ разрешить предоставление `DbConnection` извне — это прекратить использование метода `DbContext.OnConfiguring`, чтобы настроить контекст и создать параметры `DbContextOptions` извне и передать их конструктору контекста.

> [!TIP]
> `DbContextOptionsBuilder` — это API, который вы использовали в `DbContext.OnConfiguring` для настройки контекста. Теперь вы будете использовать его извне для создания параметров `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction.cs?name=Context&highlight=3,4,5)]

Как вариант, можно использовать `DbContext.OnConfiguring`, но принимать подключение `DbConnection`, которое сохраняется, а затем используется в `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Совместное использование подключения и транзакции

Теперь вы можете создать несколько экземпляров контекста, которые используют одно и то же подключение. Затем используйте API `DbContext.Database.UseTransaction(DbTransaction)`, чтобы включить оба контекста в одну транзакцию.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction.cs?name=Transaction&highlight=1-3,6,14,21-23)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Использование внешних транзакций базы данных (только для реляционных баз данных)

Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, вам может потребоваться обмениваться транзакциями между операциями, выполняемыми с помощью различных технологий.

В следующем примере показано, как выполнить операцию ADO.NET SqlClient и операцию Entity Framework Core в одной транзакции.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction.cs?name=Transaction&highlight=4,9,20,25-27)]

## <a name="using-systemtransactions"></a>Использование System.Transactions

Можно использовать внешние транзакции, если вам нужно координировать действия в более широком диапазоне.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction.cs?name=Transaction&highlight=1,2,3,26-28)]

Также можно использовать явную транзакцию.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction.cs?name=Transaction&highlight=1-2,15,28-30)]

### <a name="limitations-of-systemtransactions"></a>Ограничения System.Transactions

1. Реализация поддержки System.Transactions в EF Core зависит от поставщиков баз данных. Если поставщик не реализует поддержку System.Transactions, вызовы этих API могут полностью игнорироваться. SqlClient поддерживает этот вариант.

   > [!IMPORTANT]
   > Рекомендуется проверить, что API правильно работает с поставщиком, прежде чем использовать его для управления транзакциями. Если он работает неправильно, рекомендуется связаться с представителем поставщика базы данных.

2. Начиная с .NET Core 2.1, реализация System.Transactions не включает в себя поддержку распределенных транзакций, поэтому вы не можете использовать `TransactionScope` или `CommittableTransaction` для координации транзакций в нескольких диспетчерах ресурсов.
