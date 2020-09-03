---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 07/20/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: f822e3ae776778749a654377cbd9d9814ca40972
ms.sourcegitcommit: 12d257db4786487a0c28e9ddd79f176f7cf6edb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043601"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="4c9fe-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="4c9fe-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="4c9fe-104">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-104">EF Core 5.0 is currently in development.</span></span> <span data-ttu-id="4c9fe-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="4c9fe-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-106">This page does not duplicate the [plan for EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span></span> <span data-ttu-id="4c9fe-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="4c9fe-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-8"></a><span data-ttu-id="4c9fe-109">предварительная версия 8</span><span class="sxs-lookup"><span data-stu-id="4c9fe-109">Preview 8</span></span>

### <a name="table-per-type-tpt-mapping"></a><span data-ttu-id="4c9fe-110">Сопоставление типа "одна таблица на тип" (TPT)</span><span class="sxs-lookup"><span data-stu-id="4c9fe-110">Table-per-type (TPT) mapping</span></span>

<span data-ttu-id="4c9fe-111">По умолчанию в EF Core иерархия наследования типов .NET сопоставляется с одной таблицей базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-111">By default, EF Core maps an inheritance hierarchy of .NET types to a single database table.</span></span> <span data-ttu-id="4c9fe-112">Это называется сопоставлением "одна таблица на иерархию" (TPH).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-112">This is known as table-per-hierarchy (TPH) mapping.</span></span> <span data-ttu-id="4c9fe-113">В EF Core 5.0 каждый тип .NET в иерархии наследования также может сопоставляться с отдельной таблицей базы данных. Это называется сопоставлением "одна таблица на тип" (TPT).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-113">EF Core 5.0 also allows mapping each .NET type in an inheritance hierarchy to a different database table; known as table-per-type (TPT) mapping.</span></span>

<span data-ttu-id="4c9fe-114">Например, возьмем следующую модель с сопоставленной иерархией.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-114">For example, consider this model with a mapped hierarchy:</span></span>

```c#
public class Animal
{
    public int Id { get; set; }
    public string Species { get; set; }
}

public class Pet : Animal
{
    public string Name { get; set; }
}

public class Cat : Pet
{
    public string EdcuationLevel { get; set; }
}

public class Dog : Pet
{
    public string FavoriteToy { get; set; }
}
```

<span data-ttu-id="4c9fe-115">По умолчанию в EF Core она будет сопоставлена с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-115">By default, EF Core will map this to a single table:</span></span>

```sql
CREATE TABLE [Animals] (
    [Id] int NOT NULL IDENTITY,
    [Species] nvarchar(max) NULL,
    [Discriminator] nvarchar(max) NOT NULL,
    [Name] nvarchar(max) NULL,
    [EdcuationLevel] nvarchar(max) NULL,
    [FavoriteToy] nvarchar(max) NULL,
    CONSTRAINT [PK_Animals] PRIMARY KEY ([Id])
);
```

<span data-ttu-id="4c9fe-116">Однако сопоставление каждого типа сущности с отдельной таблицей приведет к тому, что для каждого типа будет одна таблица.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-116">However, mapping each entity type to a different table will instead result in one table per type:</span></span>

```sql
CREATE TABLE [Animals] (
    [Id] int NOT NULL IDENTITY,
    [Species] nvarchar(max) NULL,
    CONSTRAINT [PK_Animals] PRIMARY KEY ([Id])
);

CREATE TABLE [Pets] (
    [Id] int NOT NULL,
    [Name] nvarchar(max) NULL,
    CONSTRAINT [PK_Pets] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Pets_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION
);

CREATE TABLE [Cats] (
    [Id] int NOT NULL,
    [EdcuationLevel] nvarchar(max) NULL,
    CONSTRAINT [PK_Cats] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Cats_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION,
    CONSTRAINT [FK_Cats_Pets_Id] FOREIGN KEY ([Id]) REFERENCES [Pets] ([Id]) ON DELETE NO ACTION
);

CREATE TABLE [Dogs] (
    [Id] int NOT NULL,
    [FavoriteToy] nvarchar(max) NULL,
    CONSTRAINT [PK_Dogs] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Dogs_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION,
    CONSTRAINT [FK_Dogs_Pets_Id] FOREIGN KEY ([Id]) REFERENCES [Pets] ([Id]) ON DELETE NO ACTION
);
```

<span data-ttu-id="4c9fe-117">Обратите внимание, что возможность создания ограничений внешнего ключа, как показано выше, появилась после создания ветви кода для предварительной версии 8.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-117">Note that creation of the foreign key constraints shown above were added after branching the code for preview 8.</span></span>

<span data-ttu-id="4c9fe-118">Типы сущностей можно сопоставлять с отдельными таблицами с помощью атрибутов сопоставления:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-118">Entity types can be mapped to different tables using mapping attributes:</span></span>

```c#
[Table("Animals")]
public class Animal
{
    public int Id { get; set; }
    public string Species { get; set; }
}

[Table("Pets")]
public class Pet : Animal
{
    public string Name { get; set; }
}

[Table("Cats")]
public class Cat : Pet
{
    public string EdcuationLevel { get; set; }
}

[Table("Dogs")]
public class Dog : Pet
{
    public string FavoriteToy { get; set; }
}
```

<span data-ttu-id="4c9fe-119">Или с помощью конфигурации `ModelBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-119">Or using `ModelBuilder` configuration:</span></span>

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Animal>().ToTable("Animals");
    modelBuilder.Entity<Pet>().ToTable("Pets");
    modelBuilder.Entity<Cat>().ToTable("Cats");
    modelBuilder.Entity<Dog>().ToTable("Dogs");
}
```

<span data-ttu-id="4c9fe-120">Документация отслеживается по проблеме [#1979](https://github.com/dotnet/EntityFramework.Docs/issues/1979).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-120">Documentation is tracked by issue [#1979](https://github.com/dotnet/EntityFramework.Docs/issues/1979).</span></span>

### <a name="migrations-rebuild-sqlite-tables"></a><span data-ttu-id="4c9fe-121">Миграции: перестроение таблиц SQLite</span><span class="sxs-lookup"><span data-stu-id="4c9fe-121">Migrations: Rebuild SQLite tables</span></span>

<span data-ttu-id="4c9fe-122">По сравнению с другими базами данных SQLite имеет относительно ограниченные возможности работы со схемами.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-122">Compared to other database, SQLite is relatively limited in its schema manipulation capabilities.</span></span> <span data-ttu-id="4c9fe-123">Например, для удаления столбца из существующей таблицы требуется удалить и повторно создать всю таблицу.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-123">For example, dropping a column from an existing table requires that the entire table be dropped and re-created.</span></span> <span data-ttu-id="4c9fe-124">Функция миграции в EF Core 5.0 теперь поддерживает автоматическое перестроение таблицы, если этого требуют изменения схемы.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-124">EF Core 5.0 Migrations now supports automatic rebuilding the table for schema changes that require it.</span></span>

<span data-ttu-id="4c9fe-125">Например, представьте, что имеется таблица `Unicorns`, созданная для типа сущности `Unicorn`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-125">For example, imagine we have a `Unicorns` table created for a `Unicorn` entity type:</span></span>

```c#
public class Unicorn
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
```

```sql
CREATE TABLE "Unicorns" (
    "Id" INTEGER NOT NULL CONSTRAINT "PK_Unicorns" PRIMARY KEY AUTOINCREMENT,
    "Name" TEXT NULL,
    "Age" INTEGER NOT NULL
);
```

<span data-ttu-id="4c9fe-126">Но оказывается, что хранить сведения о возрасте единорога не имеет смысла, поэтому мы удалим это свойство, добавим новую миграцию и обновим базу данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-126">We then learn that storing the age of a unicorn is considered very rude, so let's remove that property, add a new migration, and update the database.</span></span> <span data-ttu-id="4c9fe-127">В EF Core 3.1 это изменение завершится ошибкой, так как столбец удалить нельзя.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-127">This update will fail when using EF Core 3.1 because the column cannot be dropped.</span></span> <span data-ttu-id="4c9fe-128">В EF Core 5.0 функция миграция перестроит таблицу.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-128">In EF Core 5.0, Migrations will instead rebuild the table:</span></span>

```sql
CREATE TABLE "ef_temp_Unicorns" (
    "Id" INTEGER NOT NULL CONSTRAINT "PK_Unicorns" PRIMARY KEY AUTOINCREMENT,
    "Name" TEXT NULL
);

INSERT INTO "ef_temp_Unicorns" ("Id", "Name")
SELECT "Id", "Name"
FROM Unicorns;

PRAGMA foreign_keys = 0;

DROP TABLE "Unicorns";

ALTER TABLE "ef_temp_Unicorns" RENAME TO "Unicorns";

PRAGMA foreign_keys = 1;
```

<span data-ttu-id="4c9fe-129">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-129">Notice that:</span></span>
* <span data-ttu-id="4c9fe-130">Для новой таблицы создается временная таблица с требуемой схемой.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-130">A temporary table is created with the desired schema for the new table</span></span>
* <span data-ttu-id="4c9fe-131">Данные копируются из текущей таблицы во временную.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-131">Data is copied from the current table into the temporary table</span></span>
* <span data-ttu-id="4c9fe-132">Принудительное применение внешнего ключа отключается.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-132">Foreign key enforcement is switched off</span></span>
* <span data-ttu-id="4c9fe-133">Текущая таблица удаляется.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-133">The current table is dropped</span></span>
* <span data-ttu-id="4c9fe-134">Временная таблица переименовывается в новую.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-134">The temporary table is renamed to be the new table</span></span>

<span data-ttu-id="4c9fe-135">Документация отслеживается по проблеме [#2583](https://github.com/dotnet/EntityFramework.Docs/issues/2583).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-135">Documentation is tracked by issue [#2583](https://github.com/dotnet/EntityFramework.Docs/issues/2583).</span></span>

### <a name="table-valued-functions"></a><span data-ttu-id="4c9fe-136">Функции с табличным значением</span><span class="sxs-lookup"><span data-stu-id="4c9fe-136">Table-valued functions</span></span>

<span data-ttu-id="4c9fe-137">Эту функцию предоставил участник сообщества [@pmiddleton](https://github.com/pmiddleton).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-137">This feature was contributed from the community by [@pmiddleton](https://github.com/pmiddleton).</span></span> <span data-ttu-id="4c9fe-138">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="4c9fe-138">Many thanks for the contribution!</span></span>

<span data-ttu-id="4c9fe-139">EF Core 5.0 превосходно поддерживает сопоставление методов .NET с функциями с табличным значением (TVF).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-139">EF Core 5.0 includes first-class support for mapping .NET methods to table-valued functions (TVFs).</span></span> <span data-ttu-id="4c9fe-140">Эти функции затем можно использовать в запросах LINQ, причем дополнительная композиция результатов функции также преобразовывается в SQL.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-140">These functions can then be used in LINQ queries where additional composition on the results of the function will also be translated to SQL.</span></span>

<span data-ttu-id="4c9fe-141">Например, рассмотрим такую функцию с табличным значением, определенную в базе данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-141">For example, consider this TVF defined in a SQL Server database:</span></span>

```sql
CREATE FUNCTION GetReports(@employeeId int)
RETURNS @reports TABLE
(
    Name nvarchar(50) NOT NULL,
    IsDeveloper bit NOT NULL
)
AS
BEGIN
    WITH cteEmployees AS
    (
        SELECT Id, Name, ManagerId, IsDeveloper
        FROM Employees
        WHERE Id = @employeeId
        UNION ALL
        SELECT e.Id, e.Name, e.ManagerId, e.IsDeveloper
        FROM Employees e
        INNER JOIN cteEmployees cteEmp ON cteEmp.Id = e.ManagerId
    )
    INSERT INTO @reports
    SELECT Name, IsDeveloper
    FROM cteEmployees
    WHERE Id != @employeeId

    RETURN
END
```

<span data-ttu-id="4c9fe-142">Для использования этой функции в модели EF Core требуются два типа сущностей:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-142">The EF Core model requires two entity types to use this TVF:</span></span>
* <span data-ttu-id="4c9fe-143">тип `Employee`, который сопоставляется с таблицей Employees обычным образом;</span><span class="sxs-lookup"><span data-stu-id="4c9fe-143">An `Employee` type that maps to the Employees table in the normal way</span></span>
* <span data-ttu-id="4c9fe-144">тип `Report`, который соответствует форме, возвращаемой функцией с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-144">A `Report` type that matches the shape returned by the TVF</span></span>

```c#
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsDeveloper { get; set; }

    public int? ManagerId { get; set; }
    public virtual Employee Manager { get; set; }
}
```

```c#
public class Report
{
    public string Name { get; set; }
    public bool IsDeveloper { get; set; }
}
```

<span data-ttu-id="4c9fe-145">Эти типы должны быть включены в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-145">These types must be included in the EF Core model:</span></span>

```c#
modelBuilder.Entity<Employee>();
modelBuilder.Entity(typeof(Report)).HasNoKey();
```

<span data-ttu-id="4c9fe-146">Обратите внимание, что класс `Report` не имеет первичного ключа и поэтому должен быть настроен в его качестве.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-146">Notice that `Report` has no primary key and so must be configured as such.</span></span>

<span data-ttu-id="4c9fe-147">Наконец, метод .NET должен быть сопоставлен с функцией с табличным значением в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-147">Finally, a .NET method must be mapped to the TVF in the database.</span></span> <span data-ttu-id="4c9fe-148">Этот метод можно определить в DbContext с помощью нового метода `FromExpression`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-148">This method can be defined on the DbContext using the new `FromExpression` method:</span></span>

```c#
public IQueryable<Report> GetReports(int managerId)
    => FromExpression(() => GetReports(managerId));
```

<span data-ttu-id="4c9fe-149">Этот метод использует параметр и возвращаемый тип, которые соответствуют приведенной выше функции с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-149">This method uses a parameter and return type that match the TVF defined above.</span></span> <span data-ttu-id="4c9fe-150">Затем метод добавляется в модель EF Core в OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-150">The method is then added to the EF Core model in OnModelCreating:</span></span>

```c#
modelBuilder.HasDbFunction(() => GetReports(default));
```

<span data-ttu-id="4c9fe-151">(Лямбда-выражение используется здесь, чтобы упростить передачу `MethodInfo` в EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-151">(Using a lambda here is an easy way to pass the `MethodInfo` to EF Core.</span></span> <span data-ttu-id="4c9fe-152">Аргументы, передаваемые в метод, игнорируются.)</span><span class="sxs-lookup"><span data-stu-id="4c9fe-152">The arguments passed to the method are ignored.)</span></span>

<span data-ttu-id="4c9fe-153">Теперь можно создавать запросы, которые вызывают `GetReports` и составляются по результатам.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-153">We can now write queries that call `GetReports` and compose over the results.</span></span> <span data-ttu-id="4c9fe-154">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-154">For example:</span></span>

```c#
from e in context.Employees
from rc in context.GetReports(e.Id)
where rc.IsDeveloper == true
select new
{
  ManagerName = e.Name,
  EmployeeName = rc.Name,
})
```

<span data-ttu-id="4c9fe-155">В SQL Server этот код преобразуется в следующий.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-155">On SQL Server, this translates to:</span></span>

```sql
SELECT [e].[Name] AS [ManagerName], [g].[Name] AS [EmployeeName]
FROM [Employees] AS [e]
CROSS APPLY [dbo].[GetReports]([e].[Id]) AS [g]
WHERE [g].[IsDeveloper] = CAST(1 AS bit)
```

<span data-ttu-id="4c9fe-156">Обратите внимание, что SQL-запрос основан на таблице `Employees`, вызывает `GetReports`, а затем добавляет дополнительное предложение WHERE на основе результатов функции.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-156">Notice that the SQL is rooted in the `Employees` table, calls `GetReports`, and then adds an additional WHERE clause on the results of the function.</span></span>

### <a name="flexible-queryupdate-mapping"></a><span data-ttu-id="4c9fe-157">Гибкое сопоставление запросов и обновлений</span><span class="sxs-lookup"><span data-stu-id="4c9fe-157">Flexible query/update mapping</span></span>

<span data-ttu-id="4c9fe-158">EF Core 5.0 позволяет сопоставлять один и тот же тип сущности с различными объектами базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-158">EF Core 5.0 allows mapping the same entity type to different database objects.</span></span> <span data-ttu-id="4c9fe-159">Этими объектами могут быть таблицы, представления или функции.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-159">These objects may be tables, views, or functions.</span></span>

<span data-ttu-id="4c9fe-160">Например, тип сущности может быть сопоставлен как с представлением базы данных, так и с таблицей базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-160">For example, an entity type can be mapped to both a database view and a database table:</span></span>

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Blog>()
        .ToTable("Blogs")
        .ToView("BlogsView");
}
```

<span data-ttu-id="4c9fe-161">По умолчанию в таком случае EF Core будет запрашивать данные из представления и отправлять обновления в таблицу.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-161">By default, EF Core will then query from the view and send updates to the table.</span></span> <span data-ttu-id="4c9fe-162">Например, выполнение следующего кода:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-162">For example, executing the following code:</span></span>

```c#
var blog = context.Set<Blog>().Single(e => e.Name == "One Unicorn");

blog.Name = "1unicorn2";

context.SaveChanges();
```

<span data-ttu-id="4c9fe-163">приводит к выполнению запроса к представлению и последующему обновлению таблицы.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-163">Results in a query against the view, and then an update to the table:</span></span>

```sql
SELECT TOP(2) [b].[Id], [b].[Name], [b].[Url]
FROM [BlogsView] AS [b]
WHERE [b].[Name] = N'One Unicorn'

SET NOCOUNT ON;
UPDATE [Blogs] SET [Name] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

### <a name="context-wide-split-query-configuration"></a><span data-ttu-id="4c9fe-164">Конфигурация с разделением запросов в пределах контекста</span><span class="sxs-lookup"><span data-stu-id="4c9fe-164">Context-wide split-query configuration</span></span>

<span data-ttu-id="4c9fe-165">Разделение запросов (см. ниже) теперь можно настроить по умолчанию для любого запроса, выполняемого DbContext.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-165">Split queries (see below) can now be configured as the default for any query executed by the DbContext.</span></span> <span data-ttu-id="4c9fe-166">Эта конфигурация доступна только для реляционных поставщиков и поэтому должна указываться в конфигурации `UseProvider`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-166">This configuration is only available for relational providers, and so must be specified as part of the `UseProvider` configuration.</span></span> <span data-ttu-id="4c9fe-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-167">For example:</span></span>

```c#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(
            Your.SqlServerConnectionString,
            b => b.UseQuerySplittingBehavior(QuerySplittingBehavior.SplitQuery));
```

<span data-ttu-id="4c9fe-168">Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-168">Documentation is tracked by issue [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span></span>

### <a name="physicaladdress-mapping"></a><span data-ttu-id="4c9fe-169">Сопоставление PhysicalAddress</span><span class="sxs-lookup"><span data-stu-id="4c9fe-169">PhysicalAddress mapping</span></span>

<span data-ttu-id="4c9fe-170">Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-170">This feature was contributed from the community by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span> <span data-ttu-id="4c9fe-171">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="4c9fe-171">Many thanks for the contribution!</span></span>

<span data-ttu-id="4c9fe-172">Стандартный [класс PhysicalAddress](/dotnet/api/system.net.networkinformation.physicaladdress) в .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-172">The standard .NET [PhysicalAddress class](/dotnet/api/system.net.networkinformation.physicaladdress) is now automatically mapped to a string column for databases that do not already have native support.</span></span> <span data-ttu-id="4c9fe-173">Дополнительные сведения см. в примерах для `IPAddress` ниже.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-173">For more information, see the examples for `IPAddress` below.</span></span>

## <a name="preview-7"></a><span data-ttu-id="4c9fe-174">предварительная версия 7</span><span class="sxs-lookup"><span data-stu-id="4c9fe-174">Preview 7</span></span>

### <a name="dbcontextfactory"></a><span data-ttu-id="4c9fe-175">DbContextFactory</span><span class="sxs-lookup"><span data-stu-id="4c9fe-175">DbContextFactory</span></span>

<span data-ttu-id="4c9fe-176">В EF Core 5.0 введены методы `AddDbContextFactory` и `AddPooledDbContextFactory` для регистрации производства с целью создания экземпляров DbContext в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-176">EF Core 5.0 introduces `AddDbContextFactory` and `AddPooledDbContextFactory` to register a factory for creating DbContext instances in the application's dependency injection (D.I.) container.</span></span> <span data-ttu-id="4c9fe-177">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-177">For example:</span></span>

```csharp
services.AddDbContextFactory<SomeDbContext>(b =>
    b.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
```

<span data-ttu-id="4c9fe-178">Службы приложений, такие как контроллеры ASP.NET Core, могут затем использовать `IDbContextFactory<TContext>` в конструкторе службы.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-178">Application services such as ASP.NET Core controllers can then depend on `IDbContextFactory<TContext>` in the service constructor.</span></span> <span data-ttu-id="4c9fe-179">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-179">For example:</span></span>

```csharp
public class MyController
{
    private readonly IDbContextFactory<SomeDbContext> _contextFactory;

    public MyController(IDbContextFactory<SomeDbContext> contextFactory)
    {
        _contextFactory = contextFactory;
    }
}
```

<span data-ttu-id="4c9fe-180">После этого по мере необходимости можно создавать и использовать экземпляры DbContext.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-180">DbContext instances can then be created and used as needed.</span></span> <span data-ttu-id="4c9fe-181">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-181">For example:</span></span>

```csharp
public void DoSomeThing()
{
    using (var context = _contextFactory.CreateDbContext())
    {
        // ...
    }
}
```

<span data-ttu-id="4c9fe-182">Обратите внимание, что созданные таким образом экземпляры DbContext _не_ управляются поставщиком служб приложения и поэтому должны удаляться приложением.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-182">Note that the DbContext instances created in this way are _not_ managed by the application's service provider and therefore must be disposed by the application.</span></span> <span data-ttu-id="4c9fe-183">Такое разделение очень полезно для приложений Blazor, в которых рекомендуется использовать `IDbContextFactory`, но может быть полезно и в других сценариях.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-183">This decoupling is very useful for Blazor applications, where using `IDbContextFactory` is recommended, but may also be useful in other scenarios.</span></span>

<span data-ttu-id="4c9fe-184">Экземпляры DbContext можно объединять в пул путем вызова `AddPooledDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-184">DbContext instances can be pooled by calling `AddPooledDbContextFactory`.</span></span> <span data-ttu-id="4c9fe-185">Объединение в пул выполняется так же, как для `AddDbContextPool`, и имеет те же ограничения.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-185">This pooling works the same way as for `AddDbContextPool`, and also has the same limitations.</span></span>

<span data-ttu-id="4c9fe-186">Документация отслеживается по проблеме [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-186">Documentation is tracked by issue [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).</span></span>

### <a name="reset-dbcontext-state"></a><span data-ttu-id="4c9fe-187">Сброс состояния DbContext</span><span class="sxs-lookup"><span data-stu-id="4c9fe-187">Reset DbContext state</span></span>

<span data-ttu-id="4c9fe-188">В EF Core 5.0 появился метод `ChangeTracker.Clear()`, который очищает DbContext всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-188">EF Core 5.0 introduces `ChangeTracker.Clear()` which clears the DbContext of all tracked entities.</span></span> <span data-ttu-id="4c9fe-189">Обычно это не требуется, если выполняется рекомендация по созданию нового экземпляра контекста с кратким сроком существования для каждой единицы работы.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-189">This should usually not be needed when using the best practice of creating a new, short-lived context instance for each unit-of-work.</span></span> <span data-ttu-id="4c9fe-190">Однако если необходимо сбросить состояние экземпляра DbContext, то использование нового метода `Clear()` является более производительным и надежным подходом, чем массовое отсоединение всех сущностей.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-190">However, if there is a need to reset the state of a DbContext instance, then using the new `Clear()` method is more performant and robust than mass-detaching all entities.</span></span>

<span data-ttu-id="4c9fe-191">Документация отслеживается по проблеме [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-191">Documentation is tracked by issue [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).</span></span>

### <a name="new-pattern-for-store-generated-defaults"></a><span data-ttu-id="4c9fe-192">Новый шаблон для значений по умолчанию, создаваемых хранилищем</span><span class="sxs-lookup"><span data-stu-id="4c9fe-192">New pattern for store-generated defaults</span></span>

<span data-ttu-id="4c9fe-193">EF Core позволяет явно задать значение для столбца, у которого может быть ограничение на значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-193">EF Core allows an explicit value to be set for a column that may also have default value constraint.</span></span> <span data-ttu-id="4c9fe-194">В качестве индикатора для этого в EF Core используется значение, принятое по умолчанию для типа свойства в CLR. Если значение отлично от значения CLR по умолчанию, оно будет вставлено. В противном случае используется значение по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-194">EF Core uses the CLR default of type property type as a sentinel for this; if the value is not the CLR default, then it is inserted, otherwise the database default is used.</span></span>

<span data-ttu-id="4c9fe-195">Это создает проблемы для типов, для которых значение CLR по умолчанию является неподходящим индикатором, особенно для свойств `bool`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-195">This creates problems for types where the CLR default is not a good sentinel--most notably, `bool` properties.</span></span> <span data-ttu-id="4c9fe-196">В таких случаях EF Core 5.0 теперь допускает значение NULL для резервного поля.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-196">EF Core 5.0 now allows the backing field to be nullable for cases like this.</span></span> <span data-ttu-id="4c9fe-197">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-197">For example:</span></span>

```csharp
public class Blog
{
    private bool? _isValid;

    public bool IsValid
    {
        get => _isValid ?? false;
        set => _isValid = value;
    }
}
```

<span data-ttu-id="4c9fe-198">Обратите внимание, что резервное поле допускает значение NULL, а общедоступное свойство — нет.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-198">Note that the backing field is nullable, but the publicly exposed property is not.</span></span> <span data-ttu-id="4c9fe-199">В результате значение индикатора может быть `null`, и это не влияет на общую контактную зону типа сущности.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-199">This allows the sentinel value to be `null` without impacting the public surface of the entity type.</span></span> <span data-ttu-id="4c9fe-200">Если `IsValid` не задается, используется значение по умолчанию для базы данных, так как значением резервного поля остается NULL.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-200">In this case, if the `IsValid` is never set, then the database default will be used since the backing field remains null.</span></span> <span data-ttu-id="4c9fe-201">Если задается `true` или `false`, это значение сохраняется явным образом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-201">If either `true` or `false` are set, then this value is saved explicitly to the database.</span></span>

<span data-ttu-id="4c9fe-202">Документация отслеживается по проблеме [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-202">Documentation is tracked by issue [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="4c9fe-203">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="4c9fe-203">Cosmos partition keys</span></span>

<span data-ttu-id="4c9fe-204">EF Core позволяет включать ключ секции Cosmos в модель EF.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-204">EF Core allows the Cosmos partition key is included in the EF model.</span></span> <span data-ttu-id="4c9fe-205">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-205">For example:</span></span>

```csharp
modelBuilder.Entity<Customer>().HasPartitionKey(b => b.AlternateKey)
```

<span data-ttu-id="4c9fe-206">Начиная с предварительной версии 7 ключ секции включается в первичный ключ типа сущности и используется для повышения производительности некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-206">Starting with preview 7, the partition key is included in the entity type's PK and is used to improved performance in some queries.</span></span>

<span data-ttu-id="4c9fe-207">Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-207">Documentation is tracked by issue [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span></span>

### <a name="cosmos-configuration"></a><span data-ttu-id="4c9fe-208">Конфигурация Cosmos</span><span class="sxs-lookup"><span data-stu-id="4c9fe-208">Cosmos configuration</span></span>

<span data-ttu-id="4c9fe-209">В EF Core 5.0 улучшена настройка Cosmos и подключений Cosmos.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-209">EF Core 5.0 improves configuration of Cosmos and Cosmos connections.</span></span>

<span data-ttu-id="4c9fe-210">Ранее в EF Core требовалось явно указывать конечную точку и ключ при подключении к базе данных Cosmos.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-210">Previously, EF Core required the end-point and key to be specified explicitly when connecting to a Cosmos database.</span></span> <span data-ttu-id="4c9fe-211">В EF Core 5.0 вместо этого можно использовать строку подключения.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-211">EF Core 5.0 allows use of a connection string instead.</span></span> <span data-ttu-id="4c9fe-212">Кроме того, в EF Core 5.0 можно явно задавать экземпляр класса WebProxy.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-212">In addition, EF Core 5.0 allows the WebProxy instance to be explicitly set.</span></span> <span data-ttu-id="4c9fe-213">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-213">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.WebProxy(myProxyInstance);
            });
```

<span data-ttu-id="4c9fe-214">Кроме того, теперь также можно настраивать множество других значений времени ожидания, ограничений и т. д.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-214">Many other timeout values, limits, etc. can now also be configured.</span></span> <span data-ttu-id="4c9fe-215">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-215">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.LimitToEndpoint();
                cosmosOptionsBuilder.RequestTimeout(requestTimeout);
                cosmosOptionsBuilder.OpenTcpConnectionTimeout(timeout);
                cosmosOptionsBuilder.IdleTcpConnectionTimeout(timeout);
                cosmosOptionsBuilder.GatewayModeMaxConnectionLimit(connectionLimit);
                cosmosOptionsBuilder.MaxTcpConnectionsPerEndpoint(connectionLimit);
                cosmosOptionsBuilder.MaxRequestsPerTcpConnection(requestLimit);
            });
```

<span data-ttu-id="4c9fe-216">Наконец, режимом соединения по умолчанию теперь является `ConnectionMode.Gateway`, что, как правило, обеспечивает большую совместимость.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-216">Finally, the default connection mode is now `ConnectionMode.Gateway`, which is generally more compatible.</span></span>

<span data-ttu-id="4c9fe-217">Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-217">Documentation is tracked by issue [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span></span>

### <a name="scaffold-dbcontext-now-singularizes"></a><span data-ttu-id="4c9fe-218">Формирование шаблонов DbContext теперь производится в форме единственного числа</span><span class="sxs-lookup"><span data-stu-id="4c9fe-218">Scaffold-DbContext now singularizes</span></span>

<span data-ttu-id="4c9fe-219">Ранее при формировании шаблона DbContext на основе существующей базы данных в EF Core создавались имена типов сущностей, совпадающие с именами таблиц в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-219">Previously when scaffolding a DbContext from an existing database, EF Core will create entity type names that match the table names in the database.</span></span> <span data-ttu-id="4c9fe-220">Например, для таблиц `People` и `Addresses` создавались типы сущностей с именами `People` и `Addresses`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-220">For example, tables `People` and `Addresses` resulted in entity types named `People` and `Addresses`.</span></span>

<span data-ttu-id="4c9fe-221">В предыдущих выпусках это поведение можно было настраивать путем регистрации службы преобразования во множественную форму.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-221">In previous releases, this behavior was configurable through registration of a pluralization service.</span></span> <span data-ttu-id="4c9fe-222">Теперь в EF Core 5.0 в качестве службы преобразования во множественную форму по умолчанию используется пакет [Humanizer](https://www.nuget.org/packages/Humanizer.Core/).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-222">Now in EF Core 5.0, the [Humanizer](https://www.nuget.org/packages/Humanizer.Core/) package is used as a default pluralization service.</span></span> <span data-ttu-id="4c9fe-223">Это означает, что таблицы `People` и `Addresses` теперь будут реконструированы в типы сущностей с именами `Person` и `Address`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-223">This means tables `People` and `Addresses` will now be reverse engineered to entity types named `Person` and `Address`.</span></span>

### <a name="savepoints"></a><span data-ttu-id="4c9fe-224">Точки сохранения</span><span class="sxs-lookup"><span data-stu-id="4c9fe-224">Savepoints</span></span>

<span data-ttu-id="4c9fe-225">EF Core теперь поддерживает [точки сохранения](/SQL/t-sql/language-elements/save-transaction-transact-sql?view=sql-server-ver15#remarks) для лучшего контроля над транзакциями, выполняющими несколько операций.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-225">EF Core now supports [savepoints](/SQL/t-sql/language-elements/save-transaction-transact-sql?view=sql-server-ver15#remarks) for greater control over transactions that execute multiple operations.</span></span>

<span data-ttu-id="4c9fe-226">Точки сохранения можно создавать, освобождать и откатывать вручную.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-226">Savepoints can be manually created, released, and rolled back.</span></span> <span data-ttu-id="4c9fe-227">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-227">For example:</span></span>

```csharp
context.Database.CreateSavepoint("MySavePoint");
```

<span data-ttu-id="4c9fe-228">Кроме того, при неудачном выполнении `SaveChanges` в EF Core теперь выполняется откат к последней точке сохранения.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-228">In addition, EF Core will now roll back to the last savepoint when executing `SaveChanges` fails.</span></span> <span data-ttu-id="4c9fe-229">Это позволяет повторно выполнять вызов SaveChanges без повторного выполнения всей транзакции.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-229">This allows SaveChanges to be re-tried without re-trying the entire transaction.</span></span>

<span data-ttu-id="4c9fe-230">Документация отслеживается по проблеме [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-230">Documentation is tracked by issue [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).</span></span>

## <a name="preview-6"></a><span data-ttu-id="4c9fe-231">предварительная версия 6</span><span class="sxs-lookup"><span data-stu-id="4c9fe-231">Preview 6</span></span>

### <a name="split-queries-for-related-collections"></a><span data-ttu-id="4c9fe-232">Разделение запросов для связанных коллекций</span><span class="sxs-lookup"><span data-stu-id="4c9fe-232">Split queries for related collections</span></span>

<span data-ttu-id="4c9fe-233">Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-233">Starting with EF Core 3.0, EF Core always generates a single SQL query for each LINQ query.</span></span> <span data-ttu-id="4c9fe-234">Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-234">This ensures consistency of the data returned within the constraints of the transaction mode in use.</span></span> <span data-ttu-id="4c9fe-235">Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-235">However, this can become very slow when the query uses `Include` or a projection to bring back multiple related collections.</span></span>

<span data-ttu-id="4c9fe-236">Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-236">EF Core 5.0 now allows a single LINQ query including related collections to be split into multiple SQL queries.</span></span> <span data-ttu-id="4c9fe-237">Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-237">This can significantly improve performance, but can result in inconsistency in the results returned if the data changes between the two queries.</span></span> <span data-ttu-id="4c9fe-238">Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-238">Serializable or snapshot transactions can be used to mitigate this and achieve consistency with split queries, but that may bring other performance costs and behavioral difference.</span></span>

#### <a name="split-queries-with-include"></a><span data-ttu-id="4c9fe-239">Разделение запросов с помощью Include</span><span class="sxs-lookup"><span data-stu-id="4c9fe-239">Split queries with Include</span></span>

<span data-ttu-id="4c9fe-240">Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-240">For example, consider a query that pulls in two levels of related collections using `Include`:</span></span>

```CSharp
var artists = context.Artists
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="4c9fe-241">Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-241">By default, EF Core will generate the following SQL when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name", "t0"."Id", "t0"."ArtistId", "t0"."Title", "t0"."Id0", "t0"."AlbumId", "t0"."Name"
FROM "Artists" AS "a"
LEFT JOIN (
    SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "t"."Id" AS "Id0", "t"."AlbumId", "t"."Name"
    FROM "Album" AS "a0"
    LEFT JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
) AS "t0" ON "a"."Id" = "t0"."ArtistId"
ORDER BY "a"."Id", "t0"."Id", "t0"."Id0"
```

<span data-ttu-id="4c9fe-242">Для изменения этого поведения можно использовать новый API `AsSplitQuery`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-242">The new `AsSplitQuery` API can be used to change this behavior.</span></span> <span data-ttu-id="4c9fe-243">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-243">For example:</span></span>

```CSharp
var artists = context.Artists
    .AsSplitQuery()
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="4c9fe-244">AsSplitQuery доступен для всех поставщиков реляционных баз данных и может использоваться в любом месте запроса так же, как AsNoTracking.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-244">AsSplitQuery is available for all relational database providers and can be used anywhere in the query, just like AsNoTracking.</span></span> <span data-ttu-id="4c9fe-245">Теперь EF Core будет создавать следующие три SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-245">EF Core will now generate the following three SQL queries:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id", "a0"."Id"

SELECT "t"."Id", "t"."AlbumId", "t"."Name", "a"."Id", "a0"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
INNER JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
ORDER BY "a"."Id", "a0"."Id"
```

<span data-ttu-id="4c9fe-246">Поддерживаются все операции в корне запроса.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-246">All operations on the query root are supported.</span></span> <span data-ttu-id="4c9fe-247">В их число входят операции OrderBy/Skip/Take, Join, FirstOrDefault и аналогичные операции выбора одного результата.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-247">This includes OrderBy/Skip/Take, Join operations, FirstOrDefault and similar single result selecting operations.</span></span>

<span data-ttu-id="4c9fe-248">Обратите внимание, что включения с фильтрацией с операциями OrderBy/Skip/Take не поддерживаются в предварительной версии 6, но они доступны в ежедневных сборках и будут включены в предварительную версию 7.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-248">Note that filtered Includes with OrderBy/Skip/Take are not supported in preview 6, but are available in the daily builds and will be included in preview 7.</span></span>

#### <a name="split-queries-with-collection-projections"></a><span data-ttu-id="4c9fe-249">Разделение запросов с помощью проекций коллекции</span><span class="sxs-lookup"><span data-stu-id="4c9fe-249">Split queries with collection projections</span></span>

<span data-ttu-id="4c9fe-250">`AsSplitQuery` можно также использовать при загрузке коллекций в проекции.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-250">`AsSplitQuery` can also be used when collections are loaded in projections.</span></span> <span data-ttu-id="4c9fe-251">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-251">For example:</span></span>

```CSharp
context.Artists
    .AsSplitQuery()
    .Select(e => new
    {
        Artist = e,
        Albums = e.Albums,
    }).ToList();
```

<span data-ttu-id="4c9fe-252">При использовании поставщика SQLite этот LINQ-запрос создает следующие два SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-252">This LINQ query generates the following two SQL queries when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id"
```

<span data-ttu-id="4c9fe-253">Обратите внимание, что поддерживается только материализация коллекции.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-253">Note that only materialization of the collection is supported.</span></span> <span data-ttu-id="4c9fe-254">Любая композиция после `e.Albums` в приведенном выше случае не приведет к разделению запроса.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-254">Any composition after `e.Albums` in above case won't result in a split query.</span></span> <span data-ttu-id="4c9fe-255">Улучшения в этой области отслеживаются по проблеме [#21234](https://github.com/dotnet/efcore/issues/21234).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-255">Improvements in this area are tracked by [#21234](https://github.com/dotnet/efcore/issues/21234).</span></span>

### <a name="indexattribute"></a><span data-ttu-id="4c9fe-256">IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="4c9fe-256">IndexAttribute</span></span>

<span data-ttu-id="4c9fe-257">Новый IndexAttribute можно поместить в тип сущности, чтобы указать индекс для одного столбца.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-257">The new IndexAttribute can be placed on an entity type to specify an index for a single column.</span></span> <span data-ttu-id="4c9fe-258">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-258">For example:</span></span>

```CSharp
[Index(nameof(FullName), IsUnique = true)]
public class User
{
    public int Id { get; set; }

    [MaxLength(128)]
    public string FullName { get; set; }
}
```

<span data-ttu-id="4c9fe-259">Для SQL Server функция миграции создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-259">For SQL Server, Migrations will then generate the following SQL:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FullName]
    ON [Users] ([FullName])
    WHERE [FullName] IS NOT NULL;
```

<span data-ttu-id="4c9fe-260">IndexAttribute можно также использовать для указания индекса, охватывающего несколько столбцов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-260">IndexAttribute can also be used to specify an index spanning multiple columns.</span></span> <span data-ttu-id="4c9fe-261">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-261">For example:</span></span>

```CSharp
[Index(nameof(FirstName), nameof(LastName), IsUnique = true)]
public class User
{
    public int Id { get; set; }

    [MaxLength(64)]
    public string FirstName { get; set; }

    [MaxLength(64)]
    public string LastName { get; set; }
}
```

<span data-ttu-id="4c9fe-262">В SQL Server будет выведен следующий результат:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-262">For SQL Server, this results in:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FirstName_LastName]
    ON [Users] ([FirstName], [LastName])
    WHERE [FirstName] IS NOT NULL AND [LastName] IS NOT NULL;
```

<span data-ttu-id="4c9fe-263">Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-263">Documentation is tracked by issue [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span></span>

### <a name="improved-query-translation-exceptions"></a><span data-ttu-id="4c9fe-264">Улучшенные исключения преобразования запросов</span><span class="sxs-lookup"><span data-stu-id="4c9fe-264">Improved query translation exceptions</span></span>

<span data-ttu-id="4c9fe-265">Мы продолжаем улучшать сообщения об исключениях, создаваемые при сбое преобразования запроса.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-265">We are continuing to improve the exception messages generated when query translation fails.</span></span> <span data-ttu-id="4c9fe-266">Например, в этом запросе используется несопоставленное свойство `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-266">For example, this query uses the unmapped property `IsSigned`:</span></span>

```CSharp
var artists = context.Artists.Where(e => e.IsSigned).ToList();
```

<span data-ttu-id="4c9fe-267">EF Core выдаст следующее исключение, указывающее на сбой преобразования из-за несопоставленного `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-267">EF Core will throw the following exception indicating that translation failed because `IsSigned` is not mapped:</span></span>

> <span data-ttu-id="4c9fe-268">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-268">Unhandled exception.</span></span> <span data-ttu-id="4c9fe-269">"System.InvalidOperationException: не удалось преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.IsSigned)".</span><span class="sxs-lookup"><span data-stu-id="4c9fe-269">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.IsSigned)' could not be translated.</span></span> <span data-ttu-id="4c9fe-270">Дополнительные сведения: не удалось преобразовать член "IsSigned" для типа сущности "Artist".</span><span class="sxs-lookup"><span data-stu-id="4c9fe-270">Additional information: Translation of member 'IsSigned' on entity type 'Artist' failed.</span></span> <span data-ttu-id="4c9fe-271">Возможно, указанный элемент не сопоставлен.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-271">Possibly the specified member is not mapped.</span></span> <span data-ttu-id="4c9fe-272">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="4c9fe-272">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="4c9fe-273">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-273">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

<span data-ttu-id="4c9fe-274">Аналогично, при попытке преобразовать сравнения строк с семантикой, зависящей от языка и региональных параметров, теперь создаются более понятные сообщения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-274">Similarly, better exception messages are now generated when attempting to translate string comparisons with culture-dependent semantics.</span></span> <span data-ttu-id="4c9fe-275">Например, этот запрос пытается использовать `StringComparison.CurrentCulture`:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-275">For example, this query attempts to use `StringComparison.CurrentCulture`:</span></span>

```CSharp
var artists = context.Artists
    .Where(e => e.Name.Equals("The Unicorns", StringComparison.CurrentCulture))
    .ToList();
```

<span data-ttu-id="4c9fe-276">Теперь EF Core выдаст следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-276">EF Core will now throw the following exception:</span></span>

> <span data-ttu-id="4c9fe-277">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-277">Unhandled exception.</span></span> <span data-ttu-id="4c9fe-278">"System.InvalidOperationException: не удается преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))".</span><span class="sxs-lookup"><span data-stu-id="4c9fe-278">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))' could not be translated.</span></span> <span data-ttu-id="4c9fe-279">Дополнительные сведения: преобразование метода "string.Equals", принимающего аргумент "StringComparison", не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-279">Additional information: Translation of 'string.Equals' method which takes 'StringComparison' argument is not supported.</span></span> <span data-ttu-id="4c9fe-280">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2129535.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-280">See https://go.microsoft.com/fwlink/?linkid=2129535 for more information.</span></span> <span data-ttu-id="4c9fe-281">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="4c9fe-281">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="4c9fe-282">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-282">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

### <a name="specify-transaction-id"></a><span data-ttu-id="4c9fe-283">Указание идентификатора транзакции</span><span class="sxs-lookup"><span data-stu-id="4c9fe-283">Specify transaction ID</span></span>

<span data-ttu-id="4c9fe-284">Эту функцию предоставил участник сообщества [@Marusyk](https://github.com/Marusyk).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-284">This feature was contributed from the community by [@Marusyk](https://github.com/Marusyk).</span></span> <span data-ttu-id="4c9fe-285">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="4c9fe-285">Many thanks for the contribution!</span></span>

<span data-ttu-id="4c9fe-286">EF Core предоставляет идентификатор транзакции для корреляции транзакций между вызовами.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-286">EF Core exposes a transaction ID for correlation of transactions across calls.</span></span> <span data-ttu-id="4c9fe-287">Этот идентификатор обычно задается в EF Core при запуске транзакции.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-287">This ID typically set by EF Core when a transaction is started.</span></span> <span data-ttu-id="4c9fe-288">Если транзакцию запускает приложение, эта функция позволяет приложению явно задать идентификатор транзакции для его правильной корреляции везде, где она используется.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-288">If the application starts the transaction instead, then this feature allows the application to explicitly set the transaction ID so it is correlated correctly everywhere it is used.</span></span> <span data-ttu-id="4c9fe-289">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-289">For example:</span></span>

```CSharp
using (context.Database.UseTransaction(myTransaction, myId))
{
   ...
}
```

### <a name="ipaddress-mapping"></a><span data-ttu-id="4c9fe-290">Сопоставление IPAddress</span><span class="sxs-lookup"><span data-stu-id="4c9fe-290">IPAddress mapping</span></span>

<span data-ttu-id="4c9fe-291">Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-291">This feature was contributed from the community by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span> <span data-ttu-id="4c9fe-292">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="4c9fe-292">Many thanks for the contribution!</span></span>

<span data-ttu-id="4c9fe-293">Стандартный класс [IPAddress](/dotnet/api/system.net.ipaddress) .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-293">The standard .NET [IPAddress class](/dotnet/api/system.net.ipaddress) is now automatically mapped to a string column for databases that do not already have native support.</span></span> <span data-ttu-id="4c9fe-294">Например, рассмотрим сопоставление этого типа сущности:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-294">For example, consider mapping this entity type:</span></span>

```CSharp
public class Host
{
    public int Id { get; set; }
    public IPAddress Address { get; set; }
}
```

<span data-ttu-id="4c9fe-295">В SQL Server функция миграции создаст следующую таблицу:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-295">On SQL Server, this will result in Migrations creating the following table:</span></span>

```sql
CREATE TABLE [Host] (
    [Id] int NOT NULL,
    [Address] nvarchar(45) NULL,
    CONSTRAINT [PK_Host] PRIMARY KEY ([Id]));
```

<span data-ttu-id="4c9fe-296">Затем можно добавить сущности обычным образом:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-296">Entities can then be added in the normal way:</span></span>

```CSharp
context.AddRange(
    new Host { Address = IPAddress.Parse("127.0.0.1")},
    new Host { Address = IPAddress.Parse("0000:0000:0000:0000:0000:0000:0000:0001")});
```

<span data-ttu-id="4c9fe-297">Результирующий SQL вставит нормализованный IPv4- или IPv6-адрес:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-297">And the resulting SQL will insert the normalized IPv4 or IPv6 address:</span></span>

```sql
Executed DbCommand (14ms) [Parameters=[@p0='1', @p1='127.0.0.1' (Size = 45), @p2='2', @p3='::1' (Size = 45)], CommandType='Text', CommandTimeout='30']
      SET NOCOUNT ON;
      INSERT INTO [Host] ([Id], [Address])
      VALUES (@p0, @p1), (@p2, @p3);
```

### <a name="exclude-onconfiguring-when-scaffolding"></a><span data-ttu-id="4c9fe-298">Исключение OnConfiguring при формировании шаблонов</span><span class="sxs-lookup"><span data-stu-id="4c9fe-298">Exclude OnConfiguring when scaffolding</span></span>

<span data-ttu-id="4c9fe-299">При формировании DbContext из существующей базы данных EF Core по умолчанию создает перегрузку OnConfiguring со строкой подключения для немедленного использования контекста.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-299">When a DbContext is scaffolded from an existing database, EF Core by default creates an OnConfiguring overload with a connection string so that the context is immediately usable.</span></span> <span data-ttu-id="4c9fe-300">Однако это бесполезно, если у вас уже есть разделяемый класс с OnConfiguring, или если вы настраиваете контекст другим способом.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-300">However, this is not useful if you already have a partial class with OnConfiguring, or if you are configuring the context some other way.</span></span>

<span data-ttu-id="4c9fe-301">Теперь командам формирования шаблонов можно указать пропустить создание OnConfiguring.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-301">To address this, the scaffolding commands can now be instructed to omit generation of OnConfiguring.</span></span> <span data-ttu-id="4c9fe-302">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-302">For example:</span></span>

```
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer --no-onconfiguring
```

<span data-ttu-id="4c9fe-303">Или в консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-303">Or in the Package Manager Console:</span></span>

```
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer -NoOnConfiguring
```

<span data-ttu-id="4c9fe-304">Обратите внимание, что рекомендуется использовать [именованную строку подключения и защищенное хранилище, такое как секреты пользователей](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-304">Note that we recommend using [a named connection string and secure storage like User Secrets](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).</span></span>

### <a name="translations-for-firstordefault-on-strings"></a><span data-ttu-id="4c9fe-305">Преобразования для FirstOrDefault в строках</span><span class="sxs-lookup"><span data-stu-id="4c9fe-305">Translations for FirstOrDefault on strings</span></span>

<span data-ttu-id="4c9fe-306">Эту функцию предоставил участник сообщества [@dvoreckyaa](https://github.com/dvoreckyaa).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-306">This feature was contributed from the community by [@dvoreckyaa](https://github.com/dvoreckyaa).</span></span> <span data-ttu-id="4c9fe-307">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="4c9fe-307">Many thanks for the contribution!</span></span>

<span data-ttu-id="4c9fe-308">FirstOrDefault и аналогичные операторы для символов в строках теперь можно преобразовывать.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-308">FirstOrDefault and similar operators for characters in strings are now translated.</span></span> <span data-ttu-id="4c9fe-309">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-309">For example, this LINQ query:</span></span>

```CSharp
context.Customers.Where(c => c.ContactName.FirstOrDefault() == 'A').ToList();
```

<span data-ttu-id="4c9fe-310">будет преобразован в следующий SQL при использовании SQL Server:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-310">Will be translated to the following SQL when using SQL Server:</span></span>

```sql
SELECT [c].[Id], [c].[ContactName]
FROM [Customer] AS [c]
WHERE SUBSTRING([c].[ContactName], 1, 1) = N'A'
```

### <a name="simplify-case-blocks"></a><span data-ttu-id="4c9fe-311">Упрощение блоков case</span><span class="sxs-lookup"><span data-stu-id="4c9fe-311">Simplify case blocks</span></span>

<span data-ttu-id="4c9fe-312">Теперь EF Core создает улучшенные запросы с блоками case.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-312">EF Core now generates better queries with CASE blocks.</span></span> <span data-ttu-id="4c9fe-313">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-313">For example, this LINQ query:</span></span>

```CSharp
context.Weapons
    .OrderBy(w => w.Name.CompareTo("Marcus' Lancer") == 0)
    .ThenBy(w => w.Id)
```

<span data-ttu-id="4c9fe-314">раньше преобразовывался в SQL Server в:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-314">Was on SQL Server formally translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN (CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END = 0) AND CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
```

<span data-ttu-id="4c9fe-315">Но теперь преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-315">But is now translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN ([w].[Name] = N'Marcus'' Lancer') AND [w].[Name] IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
```

## <a name="preview-5"></a><span data-ttu-id="4c9fe-316">Предварительная версия 5</span><span class="sxs-lookup"><span data-stu-id="4c9fe-316">Preview 5</span></span>

### <a name="database-collations"></a><span data-ttu-id="4c9fe-317">Параметры сортировки баз данных</span><span class="sxs-lookup"><span data-stu-id="4c9fe-317">Database collations</span></span>

<span data-ttu-id="4c9fe-318">Модель EF теперь позволяет указывать параметры сортировки по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-318">The default collation for a database can now be specified in the EF model.</span></span> <span data-ttu-id="4c9fe-319">Это будет передаваться в создаваемые миграции для задания параметров сортировки при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-319">This will flow through to generated migrations to set the collation when the database is created.</span></span> <span data-ttu-id="4c9fe-320">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-320">For example:</span></span>

```CSharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="4c9fe-321">Затем функция миграции формирует следующий код для создания базы данных в SQL Server:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-321">Migrations then generates the following to create the database on SQL Server:</span></span>

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

<span data-ttu-id="4c9fe-322">Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-322">The collation to use for specific database columns can also be specified.</span></span> <span data-ttu-id="4c9fe-323">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-323">For example:</span></span>

```CSharp
 modelBuilder
     .Entity<User>()
     .Property(e => e.Name)
     .UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="4c9fe-324">Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-324">For those not using migrations, collations are now reverse-engineered from the database when scaffolding a DbContext.</span></span>

<span data-ttu-id="4c9fe-325">Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-325">Finally, the `EF.Functions.Collate()` allows for ad-hoc queries using different collations.</span></span> <span data-ttu-id="4c9fe-326">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-326">For example:</span></span>

```CSharp
context.Users.Single(e => EF.Functions.Collate(e.Name, "French_CI_AS") == "Jean-Michel Jarre");
```

<span data-ttu-id="4c9fe-327">Будет создан следующий запрос для SQL Server:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-327">This will generate the following query for SQL Server:</span></span>

```sql
SELECT TOP(2) [u].[Id], [u].[Name]
FROM [Users] AS [u]
WHERE [u].[Name] COLLATE French_CI_AS = N'Jean-Michel Jarre'
```

<span data-ttu-id="4c9fe-328">Обратите внимание, что нерегламентированные параметры сортировки следует использовать с осторожностью, так как они могут негативно повлиять на производительность базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-328">Note that ad-hoc collations should be used with care as they can negatively impact database performance.</span></span>

<span data-ttu-id="4c9fe-329">Это документируется в проблеме [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-329">Documentation is tracked by issue [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span></span>

### <a name="flow-arguments-into-idesigntimedbcontextfactory"></a><span data-ttu-id="4c9fe-330">Передача аргументов в интерфейс IDesignTimeDbContextFactory</span><span class="sxs-lookup"><span data-stu-id="4c9fe-330">Flow arguments into IDesignTimeDbContextFactory</span></span>

<span data-ttu-id="4c9fe-331">Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-331">Arguments are now flowed from the command line into the `CreateDbContext` method of [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span></span> <span data-ttu-id="4c9fe-332">Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-332">For example, to indicate this is a dev build, a custom argument (e.g. `dev`) can be passed on the command line:</span></span>

```
dotnet ef migrations add two --verbose --dev
```

<span data-ttu-id="4c9fe-333">Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-333">This argument will then flow into the factory, where it can be used to control how the context is created and initialized.</span></span> <span data-ttu-id="4c9fe-334">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-334">For example:</span></span>

```CSharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args)
        => new SomeDbContext(args.Contains("--dev"));
}
```

<span data-ttu-id="4c9fe-335">Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-335">Documentation is tracked by issue [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span></span>

### <a name="no-tracking-queries-with-identity-resolution"></a><span data-ttu-id="4c9fe-336">Запросы без отслеживания с разрешением идентификаторов</span><span class="sxs-lookup"><span data-stu-id="4c9fe-336">No-tracking queries with identity resolution</span></span>

<span data-ttu-id="4c9fe-337">Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-337">No-tracking queries can now be configured to perform identity resolution.</span></span> <span data-ttu-id="4c9fe-338">Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-338">For example, the following query will create a new Blog instance for each Post, even if each Blog has the same primary key.</span></span>

```CSharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

<span data-ttu-id="4c9fe-339">Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-339">However, at the expense of usually being slightly slower and always using more memory, this query can be changed to ensure only a single Blog instance is created:</span></span>

```CSharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

<span data-ttu-id="4c9fe-340">Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-340">Note that this is only useful for no-tracking queries since all tracking queries already exhibit this behavior.</span></span> <span data-ttu-id="4c9fe-341">Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-341">Also, following API review, the `PerformIdentityResolution` syntax will be changed.</span></span> <span data-ttu-id="4c9fe-342">См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-342">See [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span></span>

<span data-ttu-id="4c9fe-343">Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-343">Documentation is tracked by issue [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span></span>

### <a name="stored-persisted-computed-columns"></a><span data-ttu-id="4c9fe-344">Сохраняемые вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="4c9fe-344">Stored (persisted) computed columns</span></span>

<span data-ttu-id="4c9fe-345">Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-345">Most databases allow computed column values to be stored after computation.</span></span> <span data-ttu-id="4c9fe-346">Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-346">While this takes up disk space, the computed column is calculated only once on update, instead of each time its value is retrieved.</span></span> <span data-ttu-id="4c9fe-347">Это также позволяет индексировать столбец для некоторых баз данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-347">This also allows the column to be indexed for some databases.</span></span>

<span data-ttu-id="4c9fe-348">EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-348">EF Core 5.0 allows computed columns to be configured as stored.</span></span> <span data-ttu-id="4c9fe-349">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-349">For example:</span></span>

```CSharp
modelBuilder
    .Entity<User>()
    .Property(e => e.SomethingComputed)
    .HasComputedColumnSql("my sql", stored: true);
```

### <a name="sqlite-computed-columns"></a><span data-ttu-id="4c9fe-350">Вычисляемые столбцы SQLite</span><span class="sxs-lookup"><span data-stu-id="4c9fe-350">SQLite computed columns</span></span>

<span data-ttu-id="4c9fe-351">EF Core теперь поддерживает вычисляемые столбцы в базах данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-351">EF Core now supports computed columns in SQLite databases.</span></span>

## <a name="preview-4"></a><span data-ttu-id="4c9fe-352">Предварительная версия 4</span><span class="sxs-lookup"><span data-stu-id="4c9fe-352">Preview 4</span></span>

### <a name="configure-database-precisionscale-in-model"></a><span data-ttu-id="4c9fe-353">Настройка точности и масштаба для базы данных в модели</span><span class="sxs-lookup"><span data-stu-id="4c9fe-353">Configure database precision/scale in model</span></span>

<span data-ttu-id="4c9fe-354">Теперь можно указать точность и масштаб для свойства с помощью построителя модели.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-354">Precision and scale for a property can now be specified using the model builder.</span></span> <span data-ttu-id="4c9fe-355">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-355">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

<span data-ttu-id="4c9fe-356">Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)".</span><span class="sxs-lookup"><span data-stu-id="4c9fe-356">Precision and scale can still be set via the full database type, such as "decimal(16,4)".</span></span>

<span data-ttu-id="4c9fe-357">Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-357">Documentation is tracked by issue [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span></span>

### <a name="specify-sql-server-index-fill-factor"></a><span data-ttu-id="4c9fe-358">Указание коэффициента заполнения индекса SQL Server</span><span class="sxs-lookup"><span data-stu-id="4c9fe-358">Specify SQL Server index fill factor</span></span>

<span data-ttu-id="4c9fe-359">Теперь при создании индекса в SQL Server можно указать коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-359">The fill factor can now be specified when creating an index on SQL Server.</span></span> <span data-ttu-id="4c9fe-360">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-360">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a><span data-ttu-id="4c9fe-361">Предварительная версия 3</span><span class="sxs-lookup"><span data-stu-id="4c9fe-361">Preview 3</span></span>

### <a name="filtered-include"></a><span data-ttu-id="4c9fe-362">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="4c9fe-362">Filtered Include</span></span>

<span data-ttu-id="4c9fe-363">Метод Include теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-363">The Include method now supports filtering of the entities included.</span></span> <span data-ttu-id="4c9fe-364">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-364">For example:</span></span>

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="4c9fe-365">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-365">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="4c9fe-366">Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-366">Skip and Take can also be used to reduce the number of included entities.</span></span> <span data-ttu-id="4c9fe-367">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-367">For example:</span></span>

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
<span data-ttu-id="4c9fe-368">Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-368">This query will return blogs with at most five posts included on each blog.</span></span>

<span data-ttu-id="4c9fe-369">Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-369">See the [Include documentation](xref:core/querying/related-data#filtered-include) for full details.</span></span>

### <a name="new-modelbuilder-api-for-navigation-properties"></a><span data-ttu-id="4c9fe-370">Новый API ModelBuilder для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="4c9fe-370">New ModelBuilder API for navigation properties</span></span>

<span data-ttu-id="4c9fe-371">Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-371">Navigation properties are primarily configured when [defining relationships](xref:core/modeling/relationships).</span></span> <span data-ttu-id="4c9fe-372">Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-372">However, the new `Navigation` method can be used in the cases where navigation properties need additional configuration.</span></span> <span data-ttu-id="4c9fe-373">Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-373">For example, to set a backing field for the navigation when the field would not be found by convention:</span></span>

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

<span data-ttu-id="4c9fe-374">Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-374">Note that the `Navigation` API does not replace relationship configuration.</span></span> <span data-ttu-id="4c9fe-375">Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-375">Instead it allows additional configuration of navigation properties in already discovered or defined relationships.</span></span>

<span data-ttu-id="4c9fe-376">См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-376">See the [Configuring Navigation Properties documentation](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a><span data-ttu-id="4c9fe-377">Новые параметры командной строки для пространств имен и строк подключения</span><span class="sxs-lookup"><span data-stu-id="4c9fe-377">New command-line parameters for namespaces and connection strings</span></span>

<span data-ttu-id="4c9fe-378">Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-378">Migrations and scaffolding now allow namespaces to be specified on the command line.</span></span> <span data-ttu-id="4c9fe-379">Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-379">For example, to reverse engineer a database putting the context and model classes in different namespaces:</span></span>

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

<span data-ttu-id="4c9fe-380">Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-380">See the [Migrations](xref:core/managing-schemas/migrations/index#namespaces) and [Reverse Engineering](xref:core/managing-schemas/scaffolding#directories-and-namespaces) documentation for full details.</span></span>

---
<span data-ttu-id="4c9fe-381">Кроме того, теперь можно передать строку подключения команде `database-update`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-381">Also, a connection string can now be passed to the `database-update` command:</span></span>

```
dotnet ef database update --connection "connection string"
```

<span data-ttu-id="4c9fe-382">Подробные сведения см. в [документации по средствам](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-382">See the [Tools documentation](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update) for full details.</span></span>

<span data-ttu-id="4c9fe-383">Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-383">Equivalent parameters have also been added to the PowerShell commands used in the VS Package Manager Console.</span></span>

### <a name="enabledetailederrors-has-returned"></a><span data-ttu-id="4c9fe-384">Возвращение EnableDetailedErrors</span><span class="sxs-lookup"><span data-stu-id="4c9fe-384">EnableDetailedErrors has returned</span></span>

<span data-ttu-id="4c9fe-385">По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-385">For performance reasons, EF doesn't do additional null-checks when reading values from the database.</span></span> <span data-ttu-id="4c9fe-386">Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-386">This can result in exceptions that are hard to root-cause when an unexpected null is encountered.</span></span>

<span data-ttu-id="4c9fe-387">При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-387">Using `EnableDetailedErrors` will add extra null checking to queries such that, for a small performance overhead, these errors are easier to trace back to a root cause.</span></span>

<span data-ttu-id="4c9fe-388">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-388">For example:</span></span>
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors
        .UseSqlServer(Your.SqlServerConnectionString);
```

<span data-ttu-id="4c9fe-389">Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-389">Documentation is tracked by issue [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="4c9fe-390">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="4c9fe-390">Cosmos partition keys</span></span>

<span data-ttu-id="4c9fe-391">Теперь можно указать в запросе ключ секции, используемый для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-391">The partition key to use for a given query can now be specified in the query.</span></span> <span data-ttu-id="4c9fe-392">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-392">For example:</span></span>

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

<span data-ttu-id="4c9fe-393">Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-393">Documentation is tracked by issue [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span></span>

### <a name="support-for-the-sql-server-datalength-function"></a><span data-ttu-id="4c9fe-394">Поддержка функции DATALENGTH SQL Server</span><span class="sxs-lookup"><span data-stu-id="4c9fe-394">Support for the SQL Server DATALENGTH function</span></span>

<span data-ttu-id="4c9fe-395">К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-395">This can be accessed using the new `EF.Functions.DataLength` method.</span></span> <span data-ttu-id="4c9fe-396">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-396">For example:</span></span>
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
```

## <a name="preview-2"></a><span data-ttu-id="4c9fe-397">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="4c9fe-397">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="4c9fe-398">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="4c9fe-398">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="4c9fe-399">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-399">A C# attribute can now be used to specify the backing field for a property.</span></span> <span data-ttu-id="4c9fe-400">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-400">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span> <span data-ttu-id="4c9fe-401">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-401">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="4c9fe-402">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-402">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="4c9fe-403">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="4c9fe-403">Complete discriminator mapping</span></span>

<span data-ttu-id="4c9fe-404">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-404">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="4c9fe-405">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-405">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span> <span data-ttu-id="4c9fe-406">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-406">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="4c9fe-407">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-407">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="4c9fe-408">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-408">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="4c9fe-409">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-409">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="4c9fe-410">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="4c9fe-410">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="4c9fe-411">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-411">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="4c9fe-412">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-412">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="4c9fe-413">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-413">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="4c9fe-414">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-414">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="4c9fe-415">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="4c9fe-415">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="4c9fe-416">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="4c9fe-416">Simple logging</span></span>

<span data-ttu-id="4c9fe-417">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-417">This feature adds functionality similar to `Database.Log` in EF6.</span></span> <span data-ttu-id="4c9fe-418">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-418">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="4c9fe-419">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-419">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="4c9fe-420">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-420">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="4c9fe-421">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="4c9fe-421">Simple way to get generated SQL</span></span>

<span data-ttu-id="4c9fe-422">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-422">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="4c9fe-423">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-423">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="4c9fe-424">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-424">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="4c9fe-425">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="4c9fe-425">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="4c9fe-426">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-426">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span> <span data-ttu-id="4c9fe-427">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-427">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="4c9fe-428">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-428">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="4c9fe-429">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="4c9fe-429">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="4c9fe-430">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-430">It is now easier to create a DbContext instance without any connection or connection string.</span></span> <span data-ttu-id="4c9fe-431">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-431">Also, the connection or connection string can now be mutated on the context instance.</span></span> <span data-ttu-id="4c9fe-432">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-432">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="4c9fe-433">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-433">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="4c9fe-434">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="4c9fe-434">Change-tracking proxies</span></span>

<span data-ttu-id="4c9fe-435">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-435">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span> <span data-ttu-id="4c9fe-436">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-436">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span> <span data-ttu-id="4c9fe-437">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-437">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="4c9fe-438">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-438">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="4c9fe-439">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="4c9fe-439">Enhanced debug views</span></span>

<span data-ttu-id="4c9fe-440">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-440">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span> <span data-ttu-id="4c9fe-441">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-441">A debug view for the Model was implemented some time ago.</span></span> <span data-ttu-id="4c9fe-442">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-442">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="4c9fe-443">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-443">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="4c9fe-444">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-444">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="4c9fe-445">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="4c9fe-445">Improved handling of database null semantics</span></span>

<span data-ttu-id="4c9fe-446">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-446">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span> <span data-ttu-id="4c9fe-447">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-447">While C# treats null as a defined value, which compares equal to any other null.</span></span> <span data-ttu-id="4c9fe-448">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-448">EF Core by default translates queries so that they use C# null semantics.</span></span> <span data-ttu-id="4c9fe-449">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-449">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="4c9fe-450">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-450">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="4c9fe-451">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="4c9fe-451">Indexer properties</span></span>

<span data-ttu-id="4c9fe-452">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-452">EF Core 5.0 supports mapping of C# indexer properties.</span></span> <span data-ttu-id="4c9fe-453">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-453">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="4c9fe-454">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-454">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="4c9fe-455">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="4c9fe-455">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="4c9fe-456">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-456">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span> <span data-ttu-id="4c9fe-457">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-457">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="4c9fe-458">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-458">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="4c9fe-459">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="4c9fe-459">IsRelational</span></span>

<span data-ttu-id="4c9fe-460">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-460">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span> <span data-ttu-id="4c9fe-461">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-461">This method can be used to test if the DbContext is using any relational database provider.</span></span> <span data-ttu-id="4c9fe-462">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-462">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="4c9fe-463">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-463">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="4c9fe-464">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="4c9fe-464">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="4c9fe-465">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-465">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span> <span data-ttu-id="4c9fe-466">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-466">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="4c9fe-467">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](/ef/core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-467">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="4c9fe-468">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-468">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="4c9fe-469">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="4c9fe-469">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="4c9fe-470">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-470">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="4c9fe-471">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-471">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="4c9fe-472">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="4c9fe-472">DateDiffWeek</span></span>
* <span data-ttu-id="4c9fe-473">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="4c9fe-473">DateFromParts</span></span>

<span data-ttu-id="4c9fe-474">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-474">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="4c9fe-475">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-475">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="4c9fe-476">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="4c9fe-476">Query translations for more byte array constructs</span></span>

<span data-ttu-id="4c9fe-477">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-477">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="4c9fe-478">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-478">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="4c9fe-479">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-479">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="4c9fe-480">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="4c9fe-480">Query translation for Reverse</span></span>

<span data-ttu-id="4c9fe-481">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-481">Queries using `Reverse` are now translated.</span></span> <span data-ttu-id="4c9fe-482">Пример:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-482">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="4c9fe-483">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-483">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="4c9fe-484">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="4c9fe-484">Query translation for bitwise operators</span></span>

<span data-ttu-id="4c9fe-485">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="4c9fe-485">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="4c9fe-486">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-486">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="4c9fe-487">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="4c9fe-487">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="4c9fe-488">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4c9fe-488">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="4c9fe-489">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="4c9fe-489">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
