---
title: Транзакции — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 4c50d6694c6678678c0af8defe2601abee923af1
ms.sourcegitcommit: 5f11a5fa5d2cde81a4e4d0d5c3a60aa74b83cbd4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2019
ms.locfileid: "54226196"
---
# <a name="using-transactions"></a>Использование транзакций

Транзакции позволяют обрабатывать несколько операций с базой данных атомарным способом. Если транзакция зафиксирована, все операции успешно применяются к базе данных. Если транзакция отменяется, ни одна из операций не применяется к базе данных.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) из репозитория GitHub.

## <a name="default-transaction-behavior"></a>Поведение транзакции по умолчанию

По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в одном вызове `SaveChanges()` применяются в транзакции. Если какое-либо из изменений завершается ошибкой, транзакция откатывается и ни одно из изменений не применяется к базе данных. Это означает, что операция `SaveChanges()` гарантированно либо будет выполнена, либо оставит базу данных без изменений, если возникла ошибка.

Для большинства приложений это поведение по умолчанию является достаточным. Транзакциями нужно управлять вручную только в том случае, если этого требует приложение.

## <a name="controlling-transactions"></a>Управление транзакциями

Вы можете использовать API `DbContext.Database` для начала, фиксации и отката транзакций. В следующем примере показаны две операции `SaveChanges()` и запрос LINQ, выполняемый в одной транзакции.

Не все поставщики баз данных поддерживают транзакции. Некоторые поставщики могут вызывать исключение или не работать при вызове API транзакций.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Межконтекстная транзакция (только для реляционных баз данных)

Вы можете использовать транзакцию в нескольких экземплярах контекста. Эта функция доступна только при использовании поставщика реляционной базы данных, так как для этого требуется использование транзакции `DbTransaction` и подключения `DbConnection`, которые относятся к реляционным базам данных.

Для совместного использования транзакции в контекстах должны применяться одинаковые `DbConnection` и `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Разрешение подключения извне

Для совместного использования подключения `DbConnection` требуется возможность передать подключение в контекст при его создании.

Самый простой способ разрешить предоставление `DbConnection` извне — это прекратить использование метода `DbContext.OnConfiguring`, чтобы настроить контекст и создать параметры `DbContextOptions` извне и передать их конструктору контекста.

> [!TIP]  
> `DbContextOptionsBuilder` — это API, который вы использовали в `DbContext.OnConfiguring` для настройки контекста. Теперь вы будете использовать его извне для создания параметров `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Как вариант, можно использовать `DbContext.OnConfiguring`, но принимать подключение `DbConnection`, которое сохраняется, а затем используется в `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Совместное использование подключения и транзакции

Теперь вы можете создать несколько экземпляров контекста, которые используют одно и то же подключение. Затем используйте API `DbContext.Database.UseTransaction(DbTransaction)`, чтобы включить оба контекста в одну транзакцию.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Использование внешних транзакций базы данных (только для реляционных баз данных)

Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, вам может потребоваться обмениваться транзакциями между операциями, выполняемыми с помощью различных технологий.

В следующем примере показано, как выполнить операцию ADO.NET SqlClient и операцию Entity Framework Core в одной транзакции.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Использование System.Transactions

> [!NOTE]  
> Это новая возможность в EF Core 2.1.

Можно использовать внешние транзакции, если вам нужно координировать действия в более широком диапазоне.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Также можно использовать явную транзакцию.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Ограничения System.Transactions  

1. Реализация поддержки System.Transactions в EF Core зависит от поставщиков баз данных. Хотя поддержка распространена среди поставщиков ADO.NET для .NET Framework, API только недавно был добавлен в .NET Core, поэтому на этой платформе поддержка не столь распространена. Если поставщик не реализует поддержку System.Transactions, вызовы этих API могут полностью игнорироваться. SqlClient для .NET Core поддерживает эту функцию, начиная с версии 2.1. SqlClient для .NET Core 2.0 вызовет исключение, если вы попытаетесь использовать эту функцию. 

   > [!IMPORTANT]  
   > Рекомендуется проверить, что API правильно работает с поставщиком, прежде чем использовать его для управления транзакциями. Если он работает неправильно, рекомендуется связаться с представителем поставщика базы данных. 

2. Начиная с версии 2.1, реализация System.Transactions в .NET Core не включает в себя поддержку распределенных транзакций, поэтому вы не можете использовать `TransactionScope` или `CommittableTransaction` для координации транзакций в нескольких диспетчерах ресурсов. 
