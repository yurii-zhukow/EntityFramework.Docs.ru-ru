---
title: "Операции - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a>Использование транзакций

Транзакции позволяют несколько операций базы данных для обработки атомарным образом. Если транзакция будет зафиксирована, все операции успешно применены к базе данных. Если откат транзакции ни одна из операций применяются к базе данных.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) из репозитория GitHub.

## <a name="default-transaction-behavior"></a>По умолчанию поведение транзакции

По умолчанию, если поставщик базы данных поддерживает транзакции, все изменения в рамках одного вызова `SaveChanges()` применяются в транзакции. Если изменения не проходят откат транзакции, и никакие изменения применяются к базе данных. Это означает, что `SaveChanges()` гарантированно полностью завершится успехом, или оставьте без изменений, если произошла ошибка базы данных.

Это поведение по умолчанию достаточно для большинства приложений. Транзакции должны управление вручную только в том случае, если требований приложения сочтут необходимыми.

## <a name="controlling-transactions"></a>Управление транзакциями

Можно использовать `DbContext.Database` API для начала, фиксации и отката транзакций. В следующем примере показаны два `SaveChanges()` операции и LINQ запрос, выполняемый в одной транзакции.

Не все поставщики базы данных поддерживает транзакции. Некоторые поставщики могут генерировать или холостой при вызове API-интерфейсы транзакции.

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

## <a name="cross-context-transaction-relational-databases-only"></a>Кросс контекст транзакции (только реляционных баз данных)

Вы можете использовать транзакцию по нескольким экземплярам контекста. Эта функция доступна только при использовании поставщика реляционной базы данных, так как он требует использования `DbTransaction` и `DbConnection`, которой относятся к реляционным базам данных.

Для совместного использования транзакции, контексты должны совместно использовать оба `DbConnection` и `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Разрешить подключение должно предоставляться извне

Совместное использование `DbConnection` требуется возможность передавать соединение в контекст, при его создании.

Самый простой способ разрешить `DbConnection` извне, в том, — чтобы прекратить использование `DbContext.OnConfiguring` метод, чтобы настроить контекст и извне создать `DbContextOptions` и передавать их в контексте конструктора.

> [!TIP]  
> `DbContextOptionsBuilder` API-интерфейс, который использовался в `DbContext.OnConfiguring` Чтобы настроить контекст, теперь необходимо использовать его для создания внешнего `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Альтернативным вариантом является для продолжения использования `DbContext.OnConfiguring`, но принимает `DbConnection` , сохраняется, а затем использовать в `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Общий ресурс соединения и транзакции

Теперь можно создать несколько экземпляров контекст, в которых одно соединение. Затем с помощью `DbContext.Database.UseTransaction(DbTransaction)` API для прикрепления обоих контекстов в той же транзакции.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>С помощью внешних DbTransactions (только реляционных баз данных)

Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, можно совместно использовать транзакции между операции, выполняемые этими различных технологий.

Приведенный ниже показано, как выполнить операцию ADO.NET SqlClient Entity Framework Core операции в одной транзакции.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Использование System.Transactions

> [!NOTE]  
> Этот компонент впервые появился в EF Core 2.1.

Можно использовать внешней транзакции, если необходимо управлять с большей областью действия.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

Можно также прикрепить в явной транзакции.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>Ограничения System.Transactions  

1. EF Core зависит от поставщиков базы данных, чтобы обеспечить поддержку System.Transactions. Несмотря на то, что поддержка довольно часто поставщиков ADO.NET для .NET Framework, API-Интерфейс только недавно добавлено .NET Core и поэтому поддержки не является быть настолько распространена. Если поставщик не реализует поддержку System.Transactions, можно будет полностью обрабатывается вызовы этих интерфейсов API. SqlClient для .NET Core, она поддерживается с версии 2.1. SqlClient 2.0 .NET Core будет вызывать исключение из попробуйте использовать функцию. 

   > [!IMPORTANT]  
   > Рекомендуется проверить, что API правильно работает с поставщиком перед полагаться на него для управления транзакциями. Вы, рекомендуется связаться программы обслуживания поставщика базы данных, если это не так. 

2. Начиная с версии 2.1, реализация System.Transactions в .NET Core отсутствует поддержка распределенных транзакций, поэтому нельзя использовать `TransactionScope` или `CommitableTransaction`для координации транзакций через несколько диспетчеров ресурсов. 
