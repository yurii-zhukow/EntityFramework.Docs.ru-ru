---
title: Операции с настраиваемыми миграциями — EF Core
description: Управление пользовательскими и необработанными миграциями SQL для управления схемой базы данных с помощью Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 708894d8d567a4644be3a4ace98cc837465710e0
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617951"
---
# <a name="custom-migrations-operations"></a>Операции пользовательской миграции

API Мигратионбуилдер позволяет выполнять различные виды операций во время миграции, но это далеко не все. Однако API также является расширяемым, позволяя определять собственные операции. Существует два способа расширения API: с помощью `Sql()` метода или путем определения пользовательских `MigrationOperation` объектов.

Чтобы проиллюстрировать это, давайте взглянем на реализацию операции, которая создает пользователя базы данных, используя каждый из подходов. В наших переносах мы хотим включить написание следующего кода:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>Использование Мигратионбуилдер. SQL ()

Самый простой способ реализовать пользовательскую операцию — определить метод расширения, который вызывает `MigrationBuilder.Sql()` . Ниже приведен пример, в котором создается соответствующий Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Если для миграции требуется поддержка нескольких поставщиков баз данных, можно использовать `MigrationBuilder.ActiveProvider` свойство. Ниже приведен пример, поддерживающий как Microsoft SQL Server, так и PostgreSQL.

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

Этот подход работает только в том случае, если вы знакомы с каждым поставщиком, к которому будет применена пользовательская операция.

## <a name="using-a-migrationoperation"></a>Использование Мигратионоператион

Чтобы отделить пользовательскую операцию от SQL, можно определить собственную `MigrationOperation` для ее представления. Затем операция передается поставщику, чтобы он мог определить соответствующий SQL.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

При таком подходе методу расширения просто нужно добавить одну из этих операций в `MigrationBuilder.Operations` .

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

Этот подход требует, чтобы каждый поставщик знал, как создать SQL для этой операции в своей `IMigrationsSqlGenerator` службе. Ниже приведен пример переопределения генератора SQL Server для управления новой операцией.

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

Замените службу генератора SQL по умолчанию новой.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
