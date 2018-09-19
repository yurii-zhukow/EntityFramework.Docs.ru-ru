---
title: Операции пользовательских миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: d715fe0408f25eb75c3160af79bb98fc87e41b17
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284074"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="e6756-102">Миграция пользовательских операций</span><span class="sxs-lookup"><span data-stu-id="e6756-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="e6756-103">MigrationBuilder API дает возможность выполнять различные виды операций во время миграции, но это далеко не исчерпывающий.</span><span class="sxs-lookup"><span data-stu-id="e6756-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="e6756-104">Тем не менее API также является расширяемой, что позволяет определять собственные операции.</span><span class="sxs-lookup"><span data-stu-id="e6756-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="e6756-105">Существует два способа для расширения API: с помощью `Sql()` метод, или определения пользовательских `MigrationOperation` объектов.</span><span class="sxs-lookup"><span data-stu-id="e6756-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="e6756-106">Чтобы продемонстрировать, давайте взглянем на реализации операции, которая создает пользователя базы данных с помощью каждого подхода.</span><span class="sxs-lookup"><span data-stu-id="e6756-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="e6756-107">В нашей миграций нам нужно включить написание следующего кода:</span><span class="sxs-lookup"><span data-stu-id="e6756-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="e6756-108">С помощью MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="e6756-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="e6756-109">Самый простой способ реализовать пользовательскую операцию, — определить метод расширения, который вызывает `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="e6756-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="e6756-110">Ниже приведен пример, который создает соответствующие Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="e6756-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="e6756-111">Если миграции необходима поддержка нескольких поставщиков базы данных, можно использовать `MigrationBuilder.ActiveProvider` свойство.</span><span class="sxs-lookup"><span data-stu-id="e6756-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="e6756-112">Ниже приведен пример, которые поддерживают Microsoft SQL Server и PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="e6756-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

    return migrationBuilder;
}
```

<span data-ttu-id="e6756-113">Этот подход работает, только если известно, чтобы каждый поставщик применения вашей пользовательской операции.</span><span class="sxs-lookup"><span data-stu-id="e6756-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="e6756-114">С помощью MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="e6756-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="e6756-115">Для отделения пользовательской операции из SQL, можно определить собственные `MigrationOperation` для его отображения.</span><span class="sxs-lookup"><span data-stu-id="e6756-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="e6756-116">Операция затем передается поставщику, чтобы можно было определить подходящий код SQL для создания.</span><span class="sxs-lookup"><span data-stu-id="e6756-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="e6756-117">При таком подходе метод расширения просто нужно добавить одну из этих операций для `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="e6756-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="e6756-118">Этот подход требует каждому поставщику, чтобы знать, как создавать код SQL для этой операции в их `IMigrationsSqlGenerator` службы.</span><span class="sxs-lookup"><span data-stu-id="e6756-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="e6756-119">Ниже приведен пример переопределения генератор SQL Server для обработки новой операции.</span><span class="sxs-lookup"><span data-stu-id="e6756-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="e6756-120">Замените обновленный службы генератор миграций sql по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e6756-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
