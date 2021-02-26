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
# <a name="whats-new-in-ef-core-60"></a><span data-ttu-id="19b23-103">Новые возможности EF Core 6.0</span><span class="sxs-lookup"><span data-stu-id="19b23-103">What's New in EF Core 6.0</span></span>

<span data-ttu-id="19b23-104">Сейчас EF Core 6.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="19b23-104">EF Core 6.0 is currently in development.</span></span> <span data-ttu-id="19b23-105">Здесь представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="19b23-105">This contains an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="19b23-106">Эта страница не является аналогом статьи о [планировании для EF Core 6.0](xref:core/what-is-new/ef-core-6.0/plan).</span><span class="sxs-lookup"><span data-stu-id="19b23-106">This page does not duplicate the [plan for EF Core 6.0](xref:core/what-is-new/ef-core-6.0/plan).</span></span> <span data-ttu-id="19b23-107">В плане описываются общие темы для EF Core 6.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="19b23-107">The plan describes the overall themes for EF Core 6.0, including everything we are planning to include before shipping the final release.</span></span>

## <a name="ef-core-60-preview-1"></a><span data-ttu-id="19b23-108">EF Core 6.0 (предварительная версия 1)</span><span class="sxs-lookup"><span data-stu-id="19b23-108">EF Core 6.0 Preview 1</span></span>

> [!TIP]
> <span data-ttu-id="19b23-109">Вы можете запустить и отладить весь код примеров предварительной версии 1, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/NewInEFCore6).</span><span class="sxs-lookup"><span data-stu-id="19b23-109">You can run and debug into all the preview 1 samples shown below by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/NewInEFCore6).</span></span>

### <a name="unicodeattribute"></a><span data-ttu-id="19b23-110">UnicodeAttribute</span><span class="sxs-lookup"><span data-stu-id="19b23-110">UnicodeAttribute</span></span>

<span data-ttu-id="19b23-111">Проблема, рассмотренная на сайте GitHub: [#19794](https://github.com/dotnet/efcore/issues/19794).</span><span class="sxs-lookup"><span data-stu-id="19b23-111">GitHub Issue: [#19794](https://github.com/dotnet/efcore/issues/19794).</span></span> <span data-ttu-id="19b23-112">Эта функция представлена пользователем [@RaymondHuy](https://github.com/RaymondHuy).</span><span class="sxs-lookup"><span data-stu-id="19b23-112">This feature was contributed by [@RaymondHuy](https://github.com/RaymondHuy).</span></span>

<span data-ttu-id="19b23-113">Начиная с EF Core 6.0 свойство строки теперь можно сопоставить со столбцом, не поддерживающим Юникод, с помощью атрибута сопоставления _без непосредственного указания типа базы данных_.</span><span class="sxs-lookup"><span data-stu-id="19b23-113">Starting with EF Core 6.0, a string property can now be mapped to a non-Unicode column using a mapping attribute _without specifying the database type directly_.</span></span> <span data-ttu-id="19b23-114">Например, рассмотрим тип сущности `Book` со свойством для [международного стандартного книжного номера (ISBN)](https://en.wikipedia.org/wiki/International_Standard_Book_Number) в формате "ISBN 978-3-16-148410-0":</span><span class="sxs-lookup"><span data-stu-id="19b23-114">For example, consider a `Book` entity type with a property for the [International Standard Book Number (ISBN)](https://en.wikipedia.org/wiki/International_Standard_Book_Number) in the form "ISBN 978-3-16-148410-0":</span></span>

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

<span data-ttu-id="19b23-115">Поскольку ISBN не может содержать символы, отличные от Юникода, атрибут `Unicode` будет использовать строковый тип, отличный от Юникода.</span><span class="sxs-lookup"><span data-stu-id="19b23-115">Since ISBNs cannot contain any non-unicode characters, the `Unicode` attribute will cause a non-Unicode string type to be used.</span></span> <span data-ttu-id="19b23-116">Кроме того, `MaxLength` используется для ограничения размера столбца базы данных.</span><span class="sxs-lookup"><span data-stu-id="19b23-116">In addition, `MaxLength` is used to limit the size of the database column.</span></span> <span data-ttu-id="19b23-117">Например, при использовании SQL Server в результате получается столбец базы данных `varchar(22)`:</span><span class="sxs-lookup"><span data-stu-id="19b23-117">For example, when using SQL Server, this results in a database column of `varchar(22)`:</span></span>

```sql
CREATE TABLE [Book] (
    [Id] int NOT NULL IDENTITY,
    [Title] nvarchar(max) NULL,
    [Isbn] varchar(22) NULL,
    CONSTRAINT [PK_Book] PRIMARY KEY ([Id]));
```

> [!NOTE]
> <span data-ttu-id="19b23-118">По умолчанию EF Core сопоставляет свойства строк со столбцами Юникода.</span><span class="sxs-lookup"><span data-stu-id="19b23-118">EF Core maps string properties to Unicode columns by default.</span></span> <span data-ttu-id="19b23-119">Параметр `UnicodeAttribute` игнорируется, если система базы данных поддерживает только типы Юникода.</span><span class="sxs-lookup"><span data-stu-id="19b23-119">`UnicodeAttribute` is ignored when the database system supports only Unicode types.</span></span>

### <a name="precisionattribute"></a><span data-ttu-id="19b23-120">PrecisionAttribute</span><span class="sxs-lookup"><span data-stu-id="19b23-120">PrecisionAttribute</span></span>

<span data-ttu-id="19b23-121">Проблема, рассмотренная на сайте GitHub: [#17914](https://github.com/dotnet/efcore/issues/17914).</span><span class="sxs-lookup"><span data-stu-id="19b23-121">GitHub Issue: [#17914](https://github.com/dotnet/efcore/issues/17914).</span></span> <span data-ttu-id="19b23-122">Эта функция представлена пользователем [@RaymondHuy](https://github.com/RaymondHuy).</span><span class="sxs-lookup"><span data-stu-id="19b23-122">This feature was contributed by [@RaymondHuy](https://github.com/RaymondHuy).</span></span>

<span data-ttu-id="19b23-123">Точность и масштаб столбца базы данных теперь можно настроить с помощью атрибутов сопоставления _без указания типа базы данных напрямую_.</span><span class="sxs-lookup"><span data-stu-id="19b23-123">The precision and scale of a database column can now be configured using mapping attributes _without specifying the database type directly_.</span></span> <span data-ttu-id="19b23-124">Например, рассмотрим тип сущности `Product` со свойством `Price` десятичного числа:</span><span class="sxs-lookup"><span data-stu-id="19b23-124">For example, consider a `Product` entity type with a decimal `Price` property:</span></span>

<!--
    public class Product
    {
        public int Id { get; set; }

        [Precision(precision: 10, scale: 2)]
        public decimal Price { get; set; }
    }
-->
[!code-csharp[ProductEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/PrecisionAttributeSample.cs?name=ProductEntityType)]

<span data-ttu-id="19b23-125">EF Core будет сопоставлять это свойство со столбцом базы данных с точностью 10 и масштабом 2.</span><span class="sxs-lookup"><span data-stu-id="19b23-125">EF Core will map this property to a database column with precision 10 and scale 2.</span></span> <span data-ttu-id="19b23-126">Например, в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="19b23-126">For example, on SQL Server:</span></span>

```sql
CREATE TABLE [Product] (
    [Id] int NOT NULL IDENTITY,
    [Price] decimal(10,2) NOT NULL,
    CONSTRAINT [PK_Product] PRIMARY KEY ([Id]));
```

### <a name="entitytypeconfigurationattribute"></a><span data-ttu-id="19b23-127">EntityTypeConfigurationAttribute</span><span class="sxs-lookup"><span data-stu-id="19b23-127">EntityTypeConfigurationAttribute</span></span>

<span data-ttu-id="19b23-128">Проблема, рассмотренная на сайте GitHub: [#23163](https://github.com/dotnet/efcore/issues/23163).</span><span class="sxs-lookup"><span data-stu-id="19b23-128">GitHub Issue: [#23163](https://github.com/dotnet/efcore/issues/23163).</span></span> <span data-ttu-id="19b23-129">Эта функция представлена пользователем [@KaloyanIT](https://github.com/KaloyanIT).</span><span class="sxs-lookup"><span data-stu-id="19b23-129">This feature was contributed by [@KaloyanIT](https://github.com/KaloyanIT).</span></span>

<span data-ttu-id="19b23-130">Экземпляры <xref:Microsoft.EntityFrameworkCore.IEntityTypeConfiguration%601> позволяют конфигурации <xref:Microsoft.EntityFrameworkCore.ModelBuilder> для каждого типа сущности содержаться в своем собственном классе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19b23-130"><xref:Microsoft.EntityFrameworkCore.IEntityTypeConfiguration%601> instances allow <xref:Microsoft.EntityFrameworkCore.ModelBuilder> configuration for a each entity type to be contained in its own configuration class.</span></span> <span data-ttu-id="19b23-131">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-131">For example:</span></span>

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

<span data-ttu-id="19b23-132">Обычно этот класс конфигурации должен быть создан и вызван из <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="19b23-132">Normally, this configuration class must be instantiated and called into from <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="19b23-133">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-133">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    new BookConfiguration().Configure(modelBuilder.Entity<Book>());
}
```

<span data-ttu-id="19b23-134">Начиная с EF Core 6.0 `EntityTypeConfigurationAttribute` можно поместить в тип сущности, чтобы программа EF Core могла найти и использовать соответствующую конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="19b23-134">Starting with EF Core 6.0, an `EntityTypeConfigurationAttribute` can be placed on the entity type such that EF Core can find and use appropriate configuration.</span></span> <span data-ttu-id="19b23-135">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-135">For example:</span></span>

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

<span data-ttu-id="19b23-136">Этот атрибут означает, что EF Core будет использовать указанную реализацию `IEntityTypeConfiguration` при включении типа сущности `Book` в модель.</span><span class="sxs-lookup"><span data-stu-id="19b23-136">This attribute means that EF Core will use the specified `IEntityTypeConfiguration` implementation whenever the `Book` entity type is included in a model.</span></span> <span data-ttu-id="19b23-137">Для включения типа сущности в модель применяется один из обычных механизмов.</span><span class="sxs-lookup"><span data-stu-id="19b23-137">The entity type is included in a model using one of the normal mechanisms.</span></span> <span data-ttu-id="19b23-138">Например, создав свойство <xref:Microsoft.EntityFrameworkCore.DbSet%601> для типа сущности:</span><span class="sxs-lookup"><span data-stu-id="19b23-138">For example, by creating a <xref:Microsoft.EntityFrameworkCore.DbSet%601> property for the entity type:</span></span>

<!--
public class BooksContext : DbContext
{
    public DbSet<Book> Books { get; set; }

    //...
-->
[!code-csharp[DbContext](../../../../samples/core/Miscellaneous/NewInEFCore6/EntityTypeConfigurationAttributeSample.cs?name=DbContext)]

<span data-ttu-id="19b23-139">Или зарегистрировав его в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>:</span><span class="sxs-lookup"><span data-stu-id="19b23-139">Or by registering it in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Book>();
}
```

> [!NOTE]
> <span data-ttu-id="19b23-140">Типы `EntityTypeConfigurationAttribute` не будут автоматически обнаружены в сборке.</span><span class="sxs-lookup"><span data-stu-id="19b23-140">`EntityTypeConfigurationAttribute` types will not be automatically discovered in an assembly.</span></span> <span data-ttu-id="19b23-141">Типы сущностей необходимо добавить в модель до того, как атрибут будет обнаружен для этого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="19b23-141">Entity types must be added to the model before the attribute will be discovered on that entity type.</span></span>

### <a name="translate-tostring-on-sqlite"></a><span data-ttu-id="19b23-142">Преобразование ToString в SQLite</span><span class="sxs-lookup"><span data-stu-id="19b23-142">Translate ToString on SQLite</span></span>

<span data-ttu-id="19b23-143">Проблема, рассмотренная на сайте GitHub: [#17223](https://github.com/dotnet/efcore/issues/17223).</span><span class="sxs-lookup"><span data-stu-id="19b23-143">GitHub Issue: [#17223](https://github.com/dotnet/efcore/issues/17223).</span></span> <span data-ttu-id="19b23-144">Эта функция представлена пользователем [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="19b23-144">This feature was contributed by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span>

<span data-ttu-id="19b23-145">Теперь при использовании поставщика базы данных SQLite вызовы в <xref:System.Object.ToString> преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="19b23-145">Calls to <xref:System.Object.ToString> are now translated to SQL when using the SQLite database provider.</span></span> <span data-ttu-id="19b23-146">Это может быть полезно для поиска текста, включающего столбцы, отличные от строковых.</span><span class="sxs-lookup"><span data-stu-id="19b23-146">This can be useful for text searches involving non-string columns.</span></span> <span data-ttu-id="19b23-147">Например, рассмотрим тип сущности `User`, который хранит номера телефонов в виде числовых значений:</span><span class="sxs-lookup"><span data-stu-id="19b23-147">For example, consider a `User` entity type that stores phone numbers as numeric values:</span></span>

<!--
    public class User
    {
        public int Id { get; set; }
        public string Username { get; set; }
        public long PhoneNumber { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/ToStringTranslationSample.cs?name=UserEntityType)]

<span data-ttu-id="19b23-148">Для преобразования числа в строку в базе данных можно использовать `ToString`.</span><span class="sxs-lookup"><span data-stu-id="19b23-148">`ToString` can be used to convert the number to a string in the database.</span></span> <span data-ttu-id="19b23-149">Затем можно использовать эту строку с функцией, например `LIKE` для поиска чисел, соответствующих шаблону.</span><span class="sxs-lookup"><span data-stu-id="19b23-149">We can then use this string with a function such as `LIKE` to find numbers that match a pattern.</span></span> <span data-ttu-id="19b23-150">Например, чтобы найти все числа, содержащие "555":</span><span class="sxs-lookup"><span data-stu-id="19b23-150">For example, to find all numbers containing 555:</span></span>

<!--
var users = context.Users.Where(u => EF.Functions.Like(u.PhoneNumber.ToString(), "%555%")).ToList();
-->
[!code-csharp[Query](../../../../samples/core/Miscellaneous/NewInEFCore6/ToStringTranslationSample.cs?name=Query)]

<span data-ttu-id="19b23-151">Выполняется преобразование в следующий SQL при использовании базы данных SQLite:</span><span class="sxs-lookup"><span data-stu-id="19b23-151">This translates to the following SQL when using a SQLite database:</span></span>

```sql
SELECT COUNT(*)
FROM "Users" AS "u"
WHERE CAST("u"."PhoneNumber" AS TEXT) LIKE '%555%'
```

<span data-ttu-id="19b23-152">Обратите внимание, что преобразование <xref:System.Object.ToString> для SQL Server уже поддерживается в EF Core 5.0 и может также поддерживаться другими поставщиками баз данных.</span><span class="sxs-lookup"><span data-stu-id="19b23-152">Note that translation of <xref:System.Object.ToString> for SQL Server is already supported in EF Core 5.0, and may also be supported by other database providers.</span></span>

### <a name="effunctionsrandom"></a><span data-ttu-id="19b23-153">EF.Functions.Random</span><span class="sxs-lookup"><span data-stu-id="19b23-153">EF.Functions.Random</span></span>

<span data-ttu-id="19b23-154">Проблема, рассмотренная на сайте GitHub: [#16141](https://github.com/dotnet/efcore/issues/16141).</span><span class="sxs-lookup"><span data-stu-id="19b23-154">GitHub Issue: [#16141](https://github.com/dotnet/efcore/issues/16141).</span></span> <span data-ttu-id="19b23-155">Эта функция представлена пользователем [@RaymondHuy](https://github.com/RaymondHuy).</span><span class="sxs-lookup"><span data-stu-id="19b23-155">This feature was contributed by [@RaymondHuy](https://github.com/RaymondHuy).</span></span>

<span data-ttu-id="19b23-156">`EF.Functions.Random` сопоставляется с функцией базы данных, возвращающей псевдослучайное число от 0 до 1, не включая эти числа.</span><span class="sxs-lookup"><span data-stu-id="19b23-156">`EF.Functions.Random` maps to a database function returning a pseudo-random number between 0 and 1 exclusive.</span></span> <span data-ttu-id="19b23-157">Преобразования реализованы в репозитории EF Core для SQL Server, SQLite и Cosmos.</span><span class="sxs-lookup"><span data-stu-id="19b23-157">Translations have been implemented in the EF Core repo for SQL Server, SQLite, and Cosmos.</span></span> <span data-ttu-id="19b23-158">Например, рассмотрим тип сущности `User` со свойством `Popularity`:</span><span class="sxs-lookup"><span data-stu-id="19b23-158">For example, consider a `User` entity type with a `Popularity` property:</span></span>

<!--
    public class User
    {
        public int Id { get; set; }
        public string Username { get; set; }
        public int Popularity { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/RandomFunctionSample.cs?name=UserEntityType)]

<span data-ttu-id="19b23-159">`Popularity` может иметь значения от 1 до 5 включительно.</span><span class="sxs-lookup"><span data-stu-id="19b23-159">`Popularity` can have values from 1 to 5 inclusive.</span></span> <span data-ttu-id="19b23-160">С помощью `EF.Functions.Random` можно написать запрос, возвращающий всех пользователей со случайной популярностью:</span><span class="sxs-lookup"><span data-stu-id="19b23-160">Using `EF.Functions.Random` we can write a query to return all users with a randomly chosen popularity:</span></span>

<!--
var users = context.Users.Where(u => u.Popularity == (int)(EF.Functions.Random() * 5.0) + 1).ToList();
-->
[!code-csharp[Query](../../../../samples/core/Miscellaneous/NewInEFCore6/RandomFunctionSample.cs?name=Query)]

<span data-ttu-id="19b23-161">При использовании базы данных SQL Server выполняется преобразование в следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="19b23-161">This translates to the following SQL when using a SQL Server database:</span></span>

```sql
SELECT [u].[Id], [u].[Popularity], [u].[Username]
FROM [Users] AS [u]
WHERE [u].[Popularity] = (CAST((RAND() * 5.0E0) AS int) + 1)
```

### <a name="support-for-sql-server-sparse-columns"></a><span data-ttu-id="19b23-162">Поддержка разреженных столбцов SQL Server</span><span class="sxs-lookup"><span data-stu-id="19b23-162">Support for SQL Server sparse columns</span></span>

<span data-ttu-id="19b23-163">Проблема, рассмотренная на сайте GitHub: [#8023](https://github.com/dotnet/efcore/issues/8023).</span><span class="sxs-lookup"><span data-stu-id="19b23-163">GitHub Issue: [#8023](https://github.com/dotnet/efcore/issues/8023).</span></span>

<span data-ttu-id="19b23-164">[Разреженные столбцы](/sql/relational-databases/tables/use-sparse-columns) SQL Server — это обычные столбцы, оптимизированные для хранения значений NULL.</span><span class="sxs-lookup"><span data-stu-id="19b23-164">SQL Server [sparse columns](/sql/relational-databases/tables/use-sparse-columns) are ordinary columns that are optimized to store null values.</span></span> <span data-ttu-id="19b23-165">Это может быть полезно при использовании [сопоставления наследования одной таблицы на иерархию](xref:core/modeling/inheritance), когда свойства редко используемого подтипа приводят к значению NULL в столбце для большинства строк в таблице.</span><span class="sxs-lookup"><span data-stu-id="19b23-165">This can be useful when using [TPH inheritance mapping](xref:core/modeling/inheritance) where properties of a rarely used subtype will result in null column values for most rows in the table.</span></span> <span data-ttu-id="19b23-166">Например, рассмотрим класс `ForumModerator`, который является расширением `ForumUser`:</span><span class="sxs-lookup"><span data-stu-id="19b23-166">For example, consider a `ForumModerator` class that extends from `ForumUser`:</span></span>

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

<span data-ttu-id="19b23-167">Среди миллионов пользователей может быть всего несколько модераторов.</span><span class="sxs-lookup"><span data-stu-id="19b23-167">There may be millions of users, with only a handful of these being moderators.</span></span> <span data-ttu-id="19b23-168">Это означает, что здесь может оказаться целесообразным сопоставить `ForumName` как разреженный.</span><span class="sxs-lookup"><span data-stu-id="19b23-168">This means mapping the `ForumName` as sparse might make sense here.</span></span> <span data-ttu-id="19b23-169">Теперь это можно настроить с помощью `IsSparse` в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span><span class="sxs-lookup"><span data-stu-id="19b23-169">This can now be configured using `IsSparse` in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span></span> <span data-ttu-id="19b23-170">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-170">For example:</span></span>

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

<span data-ttu-id="19b23-171">Затем миграции EF Core отметят столбец как разреженный.</span><span class="sxs-lookup"><span data-stu-id="19b23-171">EF Core migrations will then mark the column as sparse.</span></span> <span data-ttu-id="19b23-172">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-172">For example:</span></span>

```sql
CREATE TABLE [ForumUser] (
    [Id] int NOT NULL IDENTITY,
    [Username] nvarchar(max) NULL,
    [Discriminator] nvarchar(max) NOT NULL,
    [ForumName] nvarchar(max) SPARSE NULL,
    CONSTRAINT [PK_ForumUser] PRIMARY KEY ([Id]));
```

> [!NOTE]
> <span data-ttu-id="19b23-173">Разреженные столбцы имеют ограничения.</span><span class="sxs-lookup"><span data-stu-id="19b23-173">Sparse columns have limitations.</span></span> <span data-ttu-id="19b23-174">Обязательно ознакомьтесь с [документацией по разреженным столбцам SQL Server](/sql/relational-databases/tables/use-sparse-columns), чтобы убедиться, что разреженные столбцы подходят для вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="19b23-174">Make sure to read the [SQL Server sparse columns documentation](/sql/relational-databases/tables/use-sparse-columns) to ensure that sparse columns are the right choice for your scenario.</span></span>

### <a name="in-memory-database-validate-required-properties-are-not-null"></a><span data-ttu-id="19b23-175">База данных в памяти: проверка того, что значения обязательных свойств не равны NULL</span><span class="sxs-lookup"><span data-stu-id="19b23-175">In-memory database: validate required properties are not null</span></span>

<span data-ttu-id="19b23-176">Проблема, рассмотренная на сайте GitHub: [#10613](https://github.com/dotnet/efcore/issues/10613).</span><span class="sxs-lookup"><span data-stu-id="19b23-176">GitHub Issue: [#10613](https://github.com/dotnet/efcore/issues/10613).</span></span> <span data-ttu-id="19b23-177">Эта функция представлена пользователем [@fagnercarvalho](https://github.com/fagnercarvalho).</span><span class="sxs-lookup"><span data-stu-id="19b23-177">This feature was contributed by [@fagnercarvalho](https://github.com/fagnercarvalho).</span></span>

<span data-ttu-id="19b23-178">База данных в памяти EF Core теперь будет вызывать исключение, если предпринимается попытка сохранить значение NULL для свойства, помеченного как обязательное.</span><span class="sxs-lookup"><span data-stu-id="19b23-178">The EF Core in-memory database will now throw an exception if an attempt is made to save a null value for a property marked as required.</span></span> <span data-ttu-id="19b23-179">Например, рассмотрим тип `User` с обязательным свойством `Username`:</span><span class="sxs-lookup"><span data-stu-id="19b23-179">For example, consider a `User` type with a required `Username` property:</span></span>

<!--
    public class User
    {
        public int Id { get; set; }

        [Required]
        public string Username { get; set; }
    }
-->
[!code-csharp[UserEntityType](../../../../samples/core/Miscellaneous/NewInEFCore6/InMemoryRequiredPropertiesSample.cs?name=UserEntityType)]

<span data-ttu-id="19b23-180">Попытка сохранить сущность со значением `Username`, равным NULL, приведет к возникновению следующего исключения:</span><span class="sxs-lookup"><span data-stu-id="19b23-180">Attempting to save an entity with a null `Username` will result in the following exception:</span></span>

> <span data-ttu-id="19b23-181">Microsoft.EntityFrameworkCore.DbUpdateException: отсутствуют обязательные свойства "{'Username'}" для экземпляра типа сущности "Пользователь" со значением ключа "{ID: 1}".</span><span class="sxs-lookup"><span data-stu-id="19b23-181">Microsoft.EntityFrameworkCore.DbUpdateException: Required properties '{'Username'}' are missing for the instance of entity type 'User' with the key value '{Id: 1}'.</span></span>

<span data-ttu-id="19b23-182">При необходимости эту проверку можно отключить.</span><span class="sxs-lookup"><span data-stu-id="19b23-182">This validation can be disabled if necessary.</span></span> <span data-ttu-id="19b23-183">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-183">For example:</span></span>

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

### <a name="improved-sql-server-translation-for-isnullorwhitespace"></a><span data-ttu-id="19b23-184">Улучшенное преобразование SQL Server для IsNullOrWhitespace</span><span class="sxs-lookup"><span data-stu-id="19b23-184">Improved SQL Server translation for IsNullOrWhitespace</span></span>

<span data-ttu-id="19b23-185">Проблема, рассмотренная на сайте GitHub: [#22916](https://github.com/dotnet/efcore/issues/22916).</span><span class="sxs-lookup"><span data-stu-id="19b23-185">GitHub Issue: [#22916](https://github.com/dotnet/efcore/issues/22916).</span></span> <span data-ttu-id="19b23-186">Эта функция представлена пользователем [@Marusyk](https://github.com/Marusyk).</span><span class="sxs-lookup"><span data-stu-id="19b23-186">This feature was contributed by [@Marusyk](https://github.com/Marusyk).</span></span>

<span data-ttu-id="19b23-187">Обратите внимание на следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="19b23-187">Consider the following query:</span></span>

<!--
        var users = context.Users.Where(
            e => string.IsNullOrWhiteSpace(e.FirstName)
                 || string.IsNullOrWhiteSpace(e.LastName)).ToList();
-->
[!code-csharp[Query](../../../../samples/core/Miscellaneous/NewInEFCore6/IsNullOrWhitespaceSample.cs?name=Query)]

<span data-ttu-id="19b23-188">В версиях, предшествующих EF Core 6.0, этот параметр преобразовывался в следующее в SQL Server:</span><span class="sxs-lookup"><span data-stu-id="19b23-188">Before EF Core 6.0, this was translated to the following on SQL Server:</span></span>

```sql
SELECT [u].[Id], [u].[FirstName], [u].[LastName]
FROM [Users] AS [u]
WHERE ([u].[FirstName] IS NULL OR (LTRIM(RTRIM([u].[FirstName])) = N'')) OR ([u].[LastName] IS NULL OR (LTRIM(RTRIM([u].[LastName])) = N''))
```

<span data-ttu-id="19b23-189">В EF Core 6.0 это преобразование было улучшено:</span><span class="sxs-lookup"><span data-stu-id="19b23-189">This translation has been improved for EF Core 6.0 to:</span></span>

```sql
SELECT [u].[Id], [u].[FirstName], [u].[LastName]
FROM [Users] AS [u]
WHERE ([u].[FirstName] IS NULL OR ([u].[FirstName] = N'')) OR ([u].[LastName] IS NULL OR ([u].[LastName] = N''))
```

### <a name="database-comments-are-scaffolded-to-code-comments"></a><span data-ttu-id="19b23-190">Комментарии к базе данных преобразуются в комментарии к коду</span><span class="sxs-lookup"><span data-stu-id="19b23-190">Database comments are scaffolded to code comments</span></span>

<span data-ttu-id="19b23-191">Проблема, рассмотренная на сайте GitHub: [#19113](https://github.com/dotnet/efcore/issues/19113).</span><span class="sxs-lookup"><span data-stu-id="19b23-191">GitHub Issue: [#19113](https://github.com/dotnet/efcore/issues/19113).</span></span> <span data-ttu-id="19b23-192">Эта функция представлена пользователем [@ErikEJ](https://github.com/ErikEJ).</span><span class="sxs-lookup"><span data-stu-id="19b23-192">This feature was contributed by [@ErikEJ](https://github.com/ErikEJ).</span></span>

<span data-ttu-id="19b23-193">Комментарии к таблицам и столбцам SQL теперь преобразуются в типы сущностей, созданных при [реконструировании модели EF Core](xref:core/managing-schemas/scaffolding) из существующей базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="19b23-193">Comments on SQL tables and columns are now scaffolded into the entity types created when [reverse-engineering an EF Core model](xref:core/managing-schemas/scaffolding) from an existing SQL Server database.</span></span> <span data-ttu-id="19b23-194">Пример.</span><span class="sxs-lookup"><span data-stu-id="19b23-194">For example:</span></span>

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

## <a name="microsoftdatasqlite-60-preview-1"></a><span data-ttu-id="19b23-195">Microsoft.Data.Sqlite 6.0 (предварительная версия 1)</span><span class="sxs-lookup"><span data-stu-id="19b23-195">Microsoft.Data.Sqlite 6.0 Preview 1</span></span>

> [!TIP]
> <span data-ttu-id="19b23-196">Вы можете запустить и отладить весь код примеров предварительной версии 1, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/NewInEFCore6).</span><span class="sxs-lookup"><span data-stu-id="19b23-196">You can run and debug into all the preview 1 samples shown below by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/NewInEFCore6).</span></span>

### <a name="savepoints-api"></a><span data-ttu-id="19b23-197">API точек сохранения</span><span class="sxs-lookup"><span data-stu-id="19b23-197">Savepoints API</span></span>

<span data-ttu-id="19b23-198">Проблема, рассмотренная на сайте GitHub: [#20228](https://github.com/dotnet/efcore/issues/20228).</span><span class="sxs-lookup"><span data-stu-id="19b23-198">GitHub Issue: [#20228](https://github.com/dotnet/efcore/issues/20228).</span></span>

<span data-ttu-id="19b23-199">Мы стандартизируем [общий API для точек сохранения в поставщиках ADO.NET](https://github.com/dotnet/runtime/issues/33397).</span><span class="sxs-lookup"><span data-stu-id="19b23-199">We have been standardizing on [a common API for savepoints in ADO.NET providers](https://github.com/dotnet/runtime/issues/33397).</span></span> <span data-ttu-id="19b23-200">Microsoft.Data.Sqlite теперь поддерживает этот API, в том числе:</span><span class="sxs-lookup"><span data-stu-id="19b23-200">Microsoft.Data.Sqlite now supports this API, including:</span></span>

- <span data-ttu-id="19b23-201"><xref:System.Data.Common.DbTransaction.Save(System.String)> для создания точки сохранения в транзакции;</span><span class="sxs-lookup"><span data-stu-id="19b23-201"><xref:System.Data.Common.DbTransaction.Save(System.String)> to create a savepoint in the transaction</span></span>
- <span data-ttu-id="19b23-202"><xref:System.Data.Common.DbTransaction.Rollback(System.String)> для отката к предыдущей точке сохранения;</span><span class="sxs-lookup"><span data-stu-id="19b23-202"><xref:System.Data.Common.DbTransaction.Rollback(System.String)> to roll back to a previous savepoint</span></span>
- <span data-ttu-id="19b23-203"><xref:System.Data.Common.DbTransaction.Release(System.String)> для освобождения точки сохранения.</span><span class="sxs-lookup"><span data-stu-id="19b23-203"><xref:System.Data.Common.DbTransaction.Release(System.String)> to release a savepoint</span></span>

<span data-ttu-id="19b23-204">Использование точки сохранения позволяет выполнить откат части транзакции без отката всей транзакции.</span><span class="sxs-lookup"><span data-stu-id="19b23-204">Using a savepoint allows part of a transaction to be rolled back without rolling back the entire transaction.</span></span> <span data-ttu-id="19b23-205">Например, приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="19b23-205">For example, the code below:</span></span>

- <span data-ttu-id="19b23-206">создает транзакцию;</span><span class="sxs-lookup"><span data-stu-id="19b23-206">Creates a transaction</span></span>
- <span data-ttu-id="19b23-207">отправляет обновление в базу данных;</span><span class="sxs-lookup"><span data-stu-id="19b23-207">Sends an update to the database</span></span>
- <span data-ttu-id="19b23-208">создает точку сохранения;</span><span class="sxs-lookup"><span data-stu-id="19b23-208">Creates a savepoint</span></span>
- <span data-ttu-id="19b23-209">отправляет другое обновление в базу данных;</span><span class="sxs-lookup"><span data-stu-id="19b23-209">Sends another update to the database</span></span>
- <span data-ttu-id="19b23-210">выполняет откат к точке сохранения, созданной ранее;</span><span class="sxs-lookup"><span data-stu-id="19b23-210">Rolls back to the savepoint previous created</span></span>
- <span data-ttu-id="19b23-211">фиксирует транзакцию.</span><span class="sxs-lookup"><span data-stu-id="19b23-211">Commits the transaction</span></span>

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

<span data-ttu-id="19b23-212">В результате первое обновление фиксируется в базе данных, а второе обновление не будет зафиксировано, поскольку для точки сохранения был выполнен откат перед фиксацией транзакции.</span><span class="sxs-lookup"><span data-stu-id="19b23-212">This will result in the first update being committed to the database, while the second update is not committed since the savepoint was rolled back before committing the transaction.</span></span>

### <a name="command-timeout-in-the-connection-string"></a><span data-ttu-id="19b23-213">Время ожидания команды в строке подключения</span><span class="sxs-lookup"><span data-stu-id="19b23-213">Command timeout in the connection string</span></span>

<span data-ttu-id="19b23-214">Проблема, рассмотренная на сайте GitHub: [#22505](https://github.com/dotnet/efcore/issues/22505).</span><span class="sxs-lookup"><span data-stu-id="19b23-214">GitHub Issue: [#22505](https://github.com/dotnet/efcore/issues/22505).</span></span> <span data-ttu-id="19b23-215">Эта функция представлена пользователем [@nmichels](https://github.com/nmichels).</span><span class="sxs-lookup"><span data-stu-id="19b23-215">This feature was contributed by [@nmichels](https://github.com/nmichels).</span></span>

<span data-ttu-id="19b23-216">Поставщики ADO.NET поддерживают два отдельных времени ожидания:</span><span class="sxs-lookup"><span data-stu-id="19b23-216">ADO.NET providers support two distinct timeouts:</span></span>

- <span data-ttu-id="19b23-217">Время ожидания подключения, которое определяет максимальное время ожидания при установке подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="19b23-217">The connection timeout, which determines the maximum time to wait when making a connection to the database.</span></span>
- <span data-ttu-id="19b23-218">Время ожидания команды, определяющее максимальное время ожидания завершения выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="19b23-218">The command timeout, which determines the maximum time to wait for a command to complete executing.</span></span>

<span data-ttu-id="19b23-219">Время ожидания команды можно задать из кода с помощью <xref:System.Data.Common.DbCommand.CommandTimeout?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="19b23-219">The command timeout can be set from code using <xref:System.Data.Common.DbCommand.CommandTimeout?displayProperty=nameWithType>.</span></span> <span data-ttu-id="19b23-220">Многие поставщики теперь также предоставляют время ожидания команды в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="19b23-220">Many providers are now also exposing this command timeout in the connection string.</span></span> <span data-ttu-id="19b23-221">Microsoft.Data.Sqlite не исключение — в нем используется ключевое слово строки подключения `Command Timeout`.</span><span class="sxs-lookup"><span data-stu-id="19b23-221">Microsoft.Data.Sqlite is following this trend with the `Command Timeout` connection string keyword.</span></span> <span data-ttu-id="19b23-222">Например, `"Command Timeout=60;DataSource=test.db"` в качестве времени ожидания по умолчанию для команд, создаваемых соединением, будет использовать 60 секунд.</span><span class="sxs-lookup"><span data-stu-id="19b23-222">For example, `"Command Timeout=60;DataSource=test.db"` will use 60 seconds as the default timeout for commands created by the connection.</span></span>

> [!TIP]
> <span data-ttu-id="19b23-223">SQLite рассматривает `Default Timeout` как синоним `Command Timeout`, что делает их взаимозаменяемыми.</span><span class="sxs-lookup"><span data-stu-id="19b23-223">Sqlite treats `Default Timeout` as a synonym for `Command Timeout` and so can be used instead if preferred.</span></span>
