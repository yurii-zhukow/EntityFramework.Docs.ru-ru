---
title: Операции с настраиваемыми миграциями — EF Core
description: Управление пользовательскими и необработанными миграциями SQL для управления схемой базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: d1d29b7789eea5e887490364a7ce3abfdc903545
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062039"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="ae32d-103">Операции пользовательской миграции</span><span class="sxs-lookup"><span data-stu-id="ae32d-103">Custom Migrations Operations</span></span>

<span data-ttu-id="ae32d-104">API Мигратионбуилдер позволяет выполнять различные виды операций во время миграции, но это далеко не все.</span><span class="sxs-lookup"><span data-stu-id="ae32d-104">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="ae32d-105">Однако API также является расширяемым, позволяя определять собственные операции.</span><span class="sxs-lookup"><span data-stu-id="ae32d-105">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="ae32d-106">Существует два способа расширения API: с помощью `Sql()` метода или путем определения пользовательских `MigrationOperation` объектов.</span><span class="sxs-lookup"><span data-stu-id="ae32d-106">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="ae32d-107">Чтобы проиллюстрировать это, давайте взглянем на реализацию операции, которая создает пользователя базы данных, используя каждый из подходов.</span><span class="sxs-lookup"><span data-stu-id="ae32d-107">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="ae32d-108">В наших переносах мы хотим включить написание следующего кода:</span><span class="sxs-lookup"><span data-stu-id="ae32d-108">In our migrations, we want to enable writing the following code:</span></span>

```csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="ae32d-109">Использование Мигратионбуилдер. SQL ()</span><span class="sxs-lookup"><span data-stu-id="ae32d-109">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="ae32d-110">Самый простой способ реализовать пользовательскую операцию — определить метод расширения, который вызывает `MigrationBuilder.Sql()` .</span><span class="sxs-lookup"><span data-stu-id="ae32d-110">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="ae32d-111">Ниже приведен пример, в котором создается соответствующий Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="ae32d-111">Here is an example that generates the appropriate Transact-SQL.</span></span>

```csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="ae32d-112">Если для миграции требуется поддержка нескольких поставщиков баз данных, можно использовать `MigrationBuilder.ActiveProvider` свойство.</span><span class="sxs-lookup"><span data-stu-id="ae32d-112">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="ae32d-113">Ниже приведен пример, поддерживающий как Microsoft SQL Server, так и PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="ae32d-113">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

```csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

<span data-ttu-id="ae32d-114">Этот подход работает только в том случае, если вы знакомы с каждым поставщиком, к которому будет применена пользовательская операция.</span><span class="sxs-lookup"><span data-stu-id="ae32d-114">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="ae32d-115">Использование Мигратионоператион</span><span class="sxs-lookup"><span data-stu-id="ae32d-115">Using a MigrationOperation</span></span>

<span data-ttu-id="ae32d-116">Чтобы отделить пользовательскую операцию от SQL, можно определить собственную `MigrationOperation` для ее представления.</span><span class="sxs-lookup"><span data-stu-id="ae32d-116">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="ae32d-117">Затем операция передается поставщику, чтобы он мог определить соответствующий SQL.</span><span class="sxs-lookup"><span data-stu-id="ae32d-117">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

```csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="ae32d-118">При таком подходе методу расширения просто нужно добавить одну из этих операций в `MigrationBuilder.Operations` .</span><span class="sxs-lookup"><span data-stu-id="ae32d-118">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

```csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

<span data-ttu-id="ae32d-119">Этот подход требует, чтобы каждый поставщик знал, как создать SQL для этой операции в своей `IMigrationsSqlGenerator` службе.</span><span class="sxs-lookup"><span data-stu-id="ae32d-119">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="ae32d-120">Ниже приведен пример переопределения генератора SQL Server для управления новой операцией.</span><span class="sxs-lookup"><span data-stu-id="ae32d-120">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

```csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="ae32d-121">Замените службу генератора SQL по умолчанию новой.</span><span class="sxs-lookup"><span data-stu-id="ae32d-121">Replace the default migrations sql generator service with the updated one.</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
