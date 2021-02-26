---
title: Новые возможности EF Core 6.0
description: Обзор новых возможностей в EF Core 6.0
author: ajcvickers
ms.date: 01/28/2021
uid: core/what-is-new/ef-core-6.0/whatsnew
ms.openlocfilehash: bcc2b3ce9047a2c6b5a89e99b96919914bcf42fe
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543202"
---
# <a name="whats-new-in-ef-core-60"></a>Новые возможности EF Core 6.0

Сейчас EF Core 6.0 находится на стадии разработки. Здесь представлен обзор интересных изменений, появившихся в каждой предварительной версии.

Эта страница не является аналогом статьи о [планировании для EF Core 6.0](xref:core/what-is-new/ef-core-6.0/plan). В плане описываются общие темы для EF Core 6.0, в том числе все, что планируется включить перед выпуском окончательной версии.

## <a name="ef-core-60-preview-1"></a>EF Core 6.0 (предварительная версия 1)

> [!TIP]
> Вы можете запустить и отладить весь код примеров предварительной версии 1, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/NewInEFCore6).

### <a name="unicodeattribute"></a>UnicodeAttribute

Проблема, рассмотренная на сайте GitHub: [#19794](https://github.com/dotnet/efcore/issues/19794). Эта функция представлена пользователем [@RaymondHuy](https://github.com/RaymondHuy).

Начиная с EF Core 6.0 свойство строки теперь можно сопоставить со столбцом, не поддерживающим Юникод, с помощью атрибута сопоставления _без непосредственного указания типа базы данных_. Например, рассмотрим тип сущности `Book` со свойством для [международного стандартного книжного номера (ISBN)](https://en.wikipedia.org/wiki/International_Standard_Book_Number) в формате "ISBN 978-3-16-148410-0":

<!--
    public class Book
    {
        public int Id { get; set; }
        public string Title { get; set; }

        [Unicode(false)]
        [MaxLength(22)]
        public string Isbn { get; set; }
    }
-->
[!code-csharp[BookEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/UnicodeAttributeSample.cs?name=BookEntityType)]

Поскольку ISBN не может содержать символы, отличные от Юникода, атрибут `Unicode` будет использовать строковый тип, отличный от Юникода. Кроме того, `MaxLength` используется для ограничения размера столбца базы данных. Например, при использовании SQL Server в результате получается столбец базы данных `varchar(22)`:

```sql
CREATE TABLE [Book] (
    [Id] int NOT NULL IDENTITY,
    [Title] nvarchar(max) NULL,
    [Isbn] varchar(22) NULL,
    CONSTRAINT [PK_Book] PRIMARY KEY ([Id]));
```

> [!NOTE]
> По умолчанию EF Core сопоставляет свойства строк со столбцами Юникода. Параметр `UnicodeAttribute` игнорируется, если система базы данных поддерживает только типы Юникода.

### <a name="precisionattribute"></a>PrecisionAttribute

Проблема, рассмотренная на сайте GitHub: [#17914](https://github.com/dotnet/efcore/issues/17914). Эта функция представлена пользователем [@RaymondHuy](https://github.com/RaymondHuy).

Точность и масштаб столбца базы данных теперь можно настроить с помощью атрибутов сопоставления _без указания типа базы данных напрямую_. Например, рассмотрим тип сущности `Product` со свойством `Price` десятичного числа:

<!--
    public class Product
    {
        public int Id { get; set; }

        [Precision(precision: 10, scale: 2)]
        public decimal Price { get; set; }
    }
-->
[!code-csharp[ProductEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/PrecisionAttributeSample.cs?name=ProductEntityType)]

EF Core будет сопоставлять это свойство со столбцом базы данных с точностью 10 и масштабом 2. Например, в SQL Server.

```sql
CREATE TABLE [Product] (
    [Id] int NOT NULL IDENTITY,
    [Price] decimal(10,2) NOT NULL,
    CONSTRAINT [PK_Product] PRIMARY KEY ([Id]));
```

### <a name="entitytypeconfigurationattribute"></a>EntityTypeConfigurationAttribute

Проблема, рассмотренная на сайте GitHub: [#23163](https://github.com/dotnet/efcore/issues/23163). Эта функция представлена пользователем [@KaloyanIT](https://github.com/KaloyanIT).

Экземпляры <xref:Microsoft.EntityFrameworkCore.IEntityTypeConfiguration%601> позволяют конфигурации <xref:Microsoft.EntityFrameworkCore.ModelBuilder> для каждого типа сущности содержаться в своем собственном классе конфигурации. Пример.

<!--
public class BookConfiguration : IEntityTypeConfiguration<Book>
{
    public void Configure(EntityTypeBuilder<Book> builder)
    {
        builder
            .Property(e => e.Isbn)
            .IsUnicode(false)
            .HasMaxLength(22);
    }
}
-->
[!code-csharp[BookConfiguration](../../../../samples/core/Miscellaneous/NewInEFCore6/EntityTypeConfigurationAttributeSample.cs?name=BookConfiguration)]

Обычно этот класс конфигурации должен быть создан и вызван из <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType>. Пример.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    new BookConfiguration().Configure(modelBuilder.Entity<Book>());
}
```

Начиная с EF Core 6.0 `EntityTypeConfigurationAttribute` можно поместить в тип сущности, чтобы программа EF Core могла найти и использовать соответствующую конфигурацию. Пример.

<!--
[EntityTypeConfiguration(typeof(BookConfiguration))]
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Isbn { get; set; }
}
-->
[!code-csharp[BookEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/EntityTypeConfigurationAttributeSample.cs?name=BookEntityType)]

Этот атрибут означает, что EF Core будет использовать указанную реализацию `IEntityTypeConfiguration` при включении типа сущности `Book` в модель. Для включения типа сущности в модель применяется один из обычных механизмов. Например, создав свойство <xref:Microsoft.EntityFrameworkCore.DbSet%601> для типа сущности:

<!--
public class BooksContext : DbContext
{
    public DbSet<Book> Books { get; set; }

    //...
-->
[!code-csharp[DbContext](../../../../samples/core/Miscellaneous/NewInEFCore6/EntityTypeConfigurationAttributeSample.cs?name=DbContext)]

Или зарегистрировав его в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Book>();
}
```

> [!NOTE]
> Типы `EntityTypeConfigurationAttribute` не будут автоматически обнаружены в сборке. Типы сущностей необходимо добавить в модель до того, как атрибут будет обнаружен для этого типа сущности.

### <a name="translate-tostring-on-sqlite"></a>Преобразование ToString в SQLite

Проблема, рассмотренная на сайте GitHub: [#17223](https://github.com/dotnet/efcore/issues/17223). Эта функция представлена пользователем [@ralmsdeveloper](https://github.com/ralmsdeveloper).

Теперь при использовании поставщика базы данных SQLite вызовы в <xref:System.Object.ToString> преобразуются в SQL. Это может быть полезно для поиска текста, включающего столбцы, отличные от строковых. Например, рассмотрим тип сущности `User`, который хранит номера телефонов в виде числовых значений:

<!--
    public class User
    {
        public int Id { get; set; }
        public string Username { get; set; }
        public long PhoneNumber { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/ToStringTranslationSample.cs?name=UserEntityType)]

Для преобразования числа в строку в базе данных можно использовать `ToString`. Затем можно использовать эту строку с функцией, например `LIKE` для поиска чисел, соответствующих шаблону. Например, чтобы найти все числа, содержащие "555":

<!--
var users = context.Users.Where(u => EF.Functions.Like(u.PhoneNumber.ToString(), "%555%")).ToList();
-->
[!code-csharp[Query](../../../../samples/core/Miscellaneous/NewInEFCore6/ToStringTranslationSample.cs?name=Query)]

Выполняется преобразование в следующий SQL при использовании базы данных SQLite:

```sql
SELECT COUNT(*)
FROM "Users" AS "u"
WHERE CAST("u"."PhoneNumber" AS TEXT) LIKE '%555%'
```

Обратите внимание, что преобразование <xref:System.Object.ToString> для SQL Server уже поддерживается в EF Core 5.0 и может также поддерживаться другими поставщиками баз данных.

### <a name="effunctionsrandom"></a>EF.Functions.Random

Проблема, рассмотренная на сайте GitHub: [#16141](https://github.com/dotnet/efcore/issues/16141). Эта функция представлена пользователем [@RaymondHuy](https://github.com/RaymondHuy).

`EF.Functions.Random` сопоставляется с функцией базы данных, возвращающей псевдослучайное число от 0 до 1, не включая эти числа. Преобразования реализованы в репозитории EF Core для SQL Server, SQLite и Cosmos. Например, рассмотрим тип сущности `User` со свойством `Popularity`:

<!--
    public class User
    {
        public int Id { get; set; }
        public string Username { get; set; }
        public int Popularity { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/RandomFunctionSample.cs?name=UserEntityType)]

`Popularity` может иметь значения от 1 до 5 включительно. С помощью `EF.Functions.Random` можно написать запрос, возвращающий всех пользователей со случайной популярностью:

<!--
var users = context.Users.Where(u => u.Popularity == (int)(EF.Functions.Random() * 5.0) + 1).ToList();
-->
[!code-csharp[Query](../../../../samples/core/Miscellaneous/NewInEFCore6/RandomFunctionSample.cs?name=Query)]

При использовании базы данных SQL Server выполняется преобразование в следующий SQL:

```sql
SELECT [u].[Id], [u].[Popularity], [u].[Username]
FROM [Users] AS [u]
WHERE [u].[Popularity] = (CAST((RAND() * 5.0E0) AS int) + 1)
```

### <a name="support-for-sql-server-sparse-columns"></a>Поддержка разреженных столбцов SQL Server

Проблема, рассмотренная на сайте GitHub: [#8023](https://github.com/dotnet/efcore/issues/8023).

[Разреженные столбцы](/sql/relational-databases/tables/use-sparse-columns) SQL Server — это обычные столбцы, оптимизированные для хранения значений NULL. Это может быть полезно при использовании [сопоставления наследования одной таблицы на иерархию](xref:core/modeling/inheritance), когда свойства редко используемого подтипа приводят к значению NULL в столбце для большинства строк в таблице. Например, рассмотрим класс `ForumModerator`, который является расширением `ForumUser`:

<!--
    public class ForumUser
    {
        public int Id { get; set; }
        public string Username { get; set; }
    }

    public class ForumModerator : ForumUser
    {
        public string ForumName { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/SparseColumnsSample.cs?name=UserEntityType)]

Среди миллионов пользователей может быть всего несколько модераторов. Это означает, что здесь может оказаться целесообразным сопоставить `ForumName` как разреженный. Теперь это можно настроить с помощью `IsSparse` в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>. Пример.

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder
                .Entity<ForumModerator>()
                .Property(e => e.ForumName)
                .IsSparse();
        }
-->
[!code-csharp[OnModelCreating](../../../../samples/core/Miscellaneous/NewInEFCore6/SparseColumnsSample.cs?name=OnModelCreating)]

Затем миграции EF Core отметят столбец как разреженный. Пример.

```sql
CREATE TABLE [ForumUser] (
    [Id] int NOT NULL IDENTITY,
    [Username] nvarchar(max) NULL,
    [Discriminator] nvarchar(max) NOT NULL,
    [ForumName] nvarchar(max) SPARSE NULL,
    CONSTRAINT [PK_ForumUser] PRIMARY KEY ([Id]));
```

> [!NOTE]
> Разреженные столбцы имеют ограничения. Обязательно ознакомьтесь с [документацией по разреженным столбцам SQL Server](/sql/relational-databases/tables/use-sparse-columns), чтобы убедиться, что разреженные столбцы подходят для вашего сценария.

### <a name="in-memory-database-validate-required-properties-are-not-null"></a>База данных в памяти: проверка того, что значения обязательных свойств не равны NULL

Проблема, рассмотренная на сайте GitHub: [#10613](https://github.com/dotnet/efcore/issues/10613). Эта функция представлена пользователем [@fagnercarvalho](https://github.com/fagnercarvalho).

База данных в памяти EF Core теперь будет вызывать исключение, если предпринимается попытка сохранить значение NULL для свойства, помеченного как обязательное. Например, рассмотрим тип `User` с обязательным свойством `Username`:

<!--
    public class User
    {
        public int Id { get; set; }

        [Required]
        public string Username { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/InMemoryRequiredPropertiesSample.cs?name=UserEntityType)]

Попытка сохранить сущность со значением `Username`, равным NULL, приведет к возникновению следующего исключения:

> Microsoft.EntityFrameworkCore.DbUpdateException: отсутствуют обязательные свойства "{'Username'}" для экземпляра типа сущности "Пользователь" со значением ключа "{ID: 1}".

При необходимости эту проверку можно отключить. Пример.

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder
                .LogTo(Console.WriteLine, new[] { InMemoryEventId.ChangesSaved })
                .UseInMemoryDatabase("UserContextWithNullCheckingDisabled")
                .EnableNullabilityCheck(false);
        }
-->
[!code-csharp[OnConfiguring](../../../../samples/core/Miscellaneous/NewInEFCore6/InMemoryRequiredPropertiesSample.cs?name=OnConfiguring)]

### <a name="improved-sql-server-translation-for-isnullorwhitespace"></a>Улучшенное преобразование SQL Server для IsNullOrWhitespace

Проблема, рассмотренная на сайте GitHub: [#22916](https://github.com/dotnet/efcore/issues/22916). Эта функция представлена пользователем [@Marusyk](https://github.com/Marusyk).

Обратите внимание на следующий запрос:

<!--
        var users = context.Users.Where(
            e => string.IsNullOrWhiteSpace(e.FirstName)
                 || string.IsNullOrWhiteSpace(e.LastName)).ToList();
-->
[!code-csharp[Query](../../../../samples/core/Miscellaneous/NewInEFCore6/IsNullOrWhitespaceSample.cs?name=Query)]

В версиях, предшествующих EF Core 6.0, этот параметр преобразовывался в следующее в SQL Server:

```sql
SELECT [u].[Id], [u].[FirstName], [u].[LastName]
FROM [Users] AS [u]
WHERE ([u].[FirstName] IS NULL OR (LTRIM(RTRIM([u].[FirstName])) = N'')) OR ([u].[LastName] IS NULL OR (LTRIM(RTRIM([u].[LastName])) = N''))
```

В EF Core 6.0 это преобразование было улучшено:

```sql
SELECT [u].[Id], [u].[FirstName], [u].[LastName]
FROM [Users] AS [u]
WHERE ([u].[FirstName] IS NULL OR ([u].[FirstName] = N'')) OR ([u].[LastName] IS NULL OR ([u].[LastName] = N''))
```

### <a name="database-comments-are-scaffolded-to-code-comments"></a>Комментарии к базе данных преобразуются в комментарии к коду

Проблема, рассмотренная на сайте GitHub: [#19113](https://github.com/dotnet/efcore/issues/19113). Эта функция представлена пользователем [@ErikEJ](https://github.com/ErikEJ).

Комментарии к таблицам и столбцам SQL теперь преобразуются в типы сущностей, созданных при [реконструировании модели EF Core](xref:core/managing-schemas/scaffolding) из существующей базы данных SQL Server. Пример.

```csharp
/// <summary>
/// The Blog table.
/// </summary>
public partial class Blog
{
    /// <summary>
    /// The primary key.
    /// </summary>
    [Key]
    public int Id { get; set; }
}
```

## <a name="microsoftdatasqlite-60-preview-1"></a>Microsoft.Data.Sqlite 6.0 (предварительная версия 1)

> [!TIP]
> Вы можете запустить и отладить весь код примеров предварительной версии 1, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/NewInEFCore6).

### <a name="savepoints-api"></a>API точек сохранения

Проблема, рассмотренная на сайте GitHub: [#20228](https://github.com/dotnet/efcore/issues/20228).

Мы стандартизируем [общий API для точек сохранения в поставщиках ADO.NET](https://github.com/dotnet/runtime/issues/33397). Microsoft.Data.Sqlite теперь поддерживает этот API, в том числе:

- <xref:System.Data.Common.DbTransaction.Save(System.String)> для создания точки сохранения в транзакции;
- <xref:System.Data.Common.DbTransaction.Rollback(System.String)> для отката к предыдущей точке сохранения;
- <xref:System.Data.Common.DbTransaction.Release(System.String)> для освобождения точки сохранения.

Использование точки сохранения позволяет выполнить откат части транзакции без отката всей транзакции. Например, приведенный ниже код:

- создает транзакцию;
- отправляет обновление в базу данных;
- создает точку сохранения;
- отправляет другое обновление в базу данных;
- выполняет откат к точке сохранения, созданной ранее;
- фиксирует транзакцию.

<!--
        using var connection = new SqliteConnection("DataSource=test.db");
        connection.Open();

        using var transaction = connection.BeginTransaction();

        using (var command = connection.CreateCommand())
        {
            command.CommandText = @"UPDATE Users SET Username = 'ajcvickers' WHERE Id = 1";
            command.ExecuteNonQuery();
        }

        transaction.Save("MySavepoint");

        using (var command = connection.CreateCommand())
        {
            command.CommandText = @"UPDATE Users SET Username = 'wfvickers' WHERE Id = 2";
            command.ExecuteNonQuery();
        }

        transaction.Rollback("MySavepoint");

        transaction.Commit();
-->
[!code-csharp[PerformUpdates](../../../../samples/core/Miscellaneous/NewInEFCore6/SqliteSamples.cs?name=PerformUpdates)]

В результате первое обновление фиксируется в базе данных, а второе обновление не будет зафиксировано, поскольку для точки сохранения был выполнен откат перед фиксацией транзакции.

### <a name="command-timeout-in-the-connection-string"></a>Время ожидания команды в строке подключения

Проблема, рассмотренная на сайте GitHub: [#22505](https://github.com/dotnet/efcore/issues/22505). Эта функция представлена пользователем [@nmichels](https://github.com/nmichels).

Поставщики ADO.NET поддерживают два отдельных времени ожидания:

- Время ожидания подключения, которое определяет максимальное время ожидания при установке подключения к базе данных.
- Время ожидания команды, определяющее максимальное время ожидания завершения выполнения команды.

Время ожидания команды можно задать из кода с помощью <xref:System.Data.Common.DbCommand.CommandTimeout?displayProperty=nameWithType>. Многие поставщики теперь также предоставляют время ожидания команды в строке подключения. Microsoft.Data.Sqlite не исключение — в нем используется ключевое слово строки подключения `Command Timeout`. Например, `"Command Timeout=60;DataSource=test.db"` в качестве времени ожидания по умолчанию для команд, создаваемых соединением, будет использовать 60 секунд.

> [!TIP]
> SQLite рассматривает `Default Timeout` как синоним `Command Timeout`, что делает их взаимозаменяемыми.
