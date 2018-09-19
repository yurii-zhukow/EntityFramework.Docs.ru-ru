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

Замените обновленный службы генератор миграций sql по умолчанию.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
