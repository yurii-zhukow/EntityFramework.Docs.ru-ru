---
title: Миграция пользовательских операций - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163146"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="7475b-102">Миграция пользовательских операций</span><span class="sxs-lookup"><span data-stu-id="7475b-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="7475b-103">MigrationBuilder API дает возможность выполнять различные виды операций во время миграции, но это далеко от exhaustive.</span><span class="sxs-lookup"><span data-stu-id="7475b-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="7475b-104">Тем не менее API-Интерфейс также является расширяемым, позволяя определять собственные операции.</span><span class="sxs-lookup"><span data-stu-id="7475b-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="7475b-105">Существует два способа для расширения API: с помощью `Sql()` метод, или определив пользовательский `MigrationOperation` объектов.</span><span class="sxs-lookup"><span data-stu-id="7475b-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="7475b-106">Чтобы проиллюстрировать это, рассмотрим реализация операции, обеспечивающий создание пользователя базы данных с помощью каждого подхода.</span><span class="sxs-lookup"><span data-stu-id="7475b-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="7475b-107">В нашем миграции требуется для включения записи в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="7475b-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="7475b-108">С помощью MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="7475b-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="7475b-109">Самый простой способ реализации пользовательской операции является определение метода расширения, который вызывает `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="7475b-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="7475b-110">Ниже приведен пример, который создает соответствующие Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="7475b-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="7475b-111">При миграции должны поддерживать несколько поставщиков базы данных, можно использовать `MigrationBuilder.ActiveProvider` свойства.</span><span class="sxs-lookup"><span data-stu-id="7475b-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="7475b-112">Ниже приведен пример поддержка Microsoft SQL Server и PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="7475b-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

``` csharp
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
}
```

<span data-ttu-id="7475b-113">Такой подход работает, только если вы знаете все поставщики которой будут применяться к пользовательской операции.</span><span class="sxs-lookup"><span data-stu-id="7475b-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="7475b-114">С помощью MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="7475b-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="7475b-115">Для отделения пользовательской операции с SQL, можно определить собственные `MigrationOperation` для его отображения.</span><span class="sxs-lookup"><span data-stu-id="7475b-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="7475b-116">Операция затем передается поставщику, чтобы можно было определить соответствующий SQL для создания.</span><span class="sxs-lookup"><span data-stu-id="7475b-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="7475b-117">При таком подходе метод расширения требуется только для одной из этих операций для добавления `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="7475b-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

``` csharp
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

<span data-ttu-id="7475b-118">Этот подход требует каждому поставщику, чтобы знать, как создавать код SQL для этой операции в их `IMigrationsSqlGenerator` службы.</span><span class="sxs-lookup"><span data-stu-id="7475b-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="7475b-119">Ниже приведен пример переопределения генератор SQL Server для обработки операции создания.</span><span class="sxs-lookup"><span data-stu-id="7475b-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

``` csharp
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
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="7475b-120">Замените обновленный служба генератора sql миграций по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7475b-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
