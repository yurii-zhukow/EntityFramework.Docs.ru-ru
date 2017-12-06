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
# <a name="using-transactions"></a>Использование транзакций

Транзакции позволяют несколько операций базы данных для обработки атомарным образом. Если транзакция будет зафиксирована, все операции успешно применены к базе данных. Если откат транзакции ни одна из операций применяются к базе данных.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) на GitHub.

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
> `DbContextOptionsBuilder`API-интерфейс, который использовался в `DbContext.OnConfiguring` Чтобы настроить контекст, теперь необходимо использовать его для создания внешнего `DbContextOptions`.

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

## <a name="using-external-dbtransactions-relational-databases-only"></a>С помощью внешних DbTransactions (только реляционных баз данных)

Если вы используете несколько технологий доступа к данным для доступа к реляционной базе данных, можно совместно использовать транзакции между операции, выполняемые этими различных технологий.

Приведенный ниже показано, как выполнить операцию ADO.NET SqlClient Entity Framework Core операции в одной транзакции.

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
