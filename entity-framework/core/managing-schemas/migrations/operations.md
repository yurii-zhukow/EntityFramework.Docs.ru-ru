---
title: "Миграция пользовательских операций - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d41409dee034e84d22092a5f9111dd79c87dcec3
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-operations"></a>Миграция пользовательских операций
============================
MigrationBuilder API дает возможность выполнять различные виды операций во время миграции, но это далеко от exhaustive. Тем не менее API-Интерфейс также является расширяемым, позволяя определять собственные операции. Существует два способа для расширения API: с помощью `Sql()` метод, или определив пользовательский `MigrationOperation` объектов.

Чтобы проиллюстрировать это, рассмотрим реализация операции, обеспечивающий создание пользователя базы данных с помощью каждого подхода. В нашем миграции требуется для включения записи в следующем примере кода:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>С помощью MigrationBuilder.Sql()
----------------------------
Самый простой способ реализации пользовательской операции является определение метода расширения, который вызывает `MigrationBuilder.Sql()`.
Ниже приведен пример, который создает соответствующие Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

При миграции должны поддерживать несколько поставщиков базы данных, можно использовать `MigrationBuilder.ActiveProvider` свойства. Ниже приведен пример поддержка Microsoft SQL Server и PostreSQL.

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

Такой подход работает, только если вы знаете все поставщики которой будут применяться к пользовательской операции.

<a name="using-a-migrationoperation"></a>С помощью MigrationOperation
---------------------------
Для отделения пользовательской операции с SQL, можно определить собственные `MigrationOperation` для его отображения. Операция затем передается поставщику, чтобы можно было определить соответствующий SQL для создания.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

При таком подходе метод расширения требуется только для одной из этих операций для добавления `MigrationBuilder.Operations`.

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

Этот подход требует каждому поставщику, чтобы знать, как создавать код SQL для этой операции в их `IMigrationsSqlGenerator` службы. Ниже приведен пример переопределения генератор SQL Server для обработки операции создания.

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

Замените обновленный служба генератора sql миграций по умолчанию.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
