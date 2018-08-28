---
title: Операции пользовательских миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 33f4fb6dca7c0d2e29abd196194a453f2b390b14
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997098"
---
<a name="custom-migrations-operations"></a>Миграция пользовательских операций
============================
MigrationBuilder API дает возможность выполнять различные виды операций во время миграции, но это далеко не исчерпывающий. Тем не менее API также является расширяемой, что позволяет определять собственные операции. Существует два способа для расширения API: с помощью `Sql()` метод, или определения пользовательских `MigrationOperation` объектов.

Чтобы продемонстрировать, давайте взглянем на реализации операции, которая создает пользователя базы данных с помощью каждого подхода. В нашей миграций нам нужно включить написание следующего кода:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>С помощью MigrationBuilder.Sql()
----------------------------
Самый простой способ реализовать пользовательскую операцию, — определить метод расширения, который вызывает `MigrationBuilder.Sql()`.
Ниже приведен пример, который создает соответствующие Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Если миграции необходима поддержка нескольких поставщиков базы данных, можно использовать `MigrationBuilder.ActiveProvider` свойство. Ниже приведен пример, которые поддерживают Microsoft SQL Server и PostgreSQL.

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

Этот подход работает, только если известно, чтобы каждый поставщик применения вашей пользовательской операции.

<a name="using-a-migrationoperation"></a>С помощью MigrationOperation
---------------------------
Для отделения пользовательской операции из SQL, можно определить собственные `MigrationOperation` для его отображения. Операция затем передается поставщику, чтобы можно было определить подходящий код SQL для создания.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

При таком подходе метод расширения просто нужно добавить одну из этих операций для `MigrationBuilder.Operations`.

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

Этот подход требует каждому поставщику, чтобы знать, как создавать код SQL для этой операции в их `IMigrationsSqlGenerator` службы. Ниже приведен пример переопределения генератор SQL Server для обработки новой операции.

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

Замените обновленный службы генератор миграций sql по умолчанию.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
