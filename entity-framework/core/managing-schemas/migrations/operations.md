---
title: Операции с настраиваемыми миграциями — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812041"
---
# <a name="custom-migrations-operations"></a>Операции пользовательской миграции

API Мигратионбуилдер позволяет выполнять различные виды операций во время миграции, но это далеко не все. Однако API также является расширяемым, позволяя определять собственные операции. Существует два способа расширения API: с помощью метода `Sql()` или путем определения пользовательских объектов `MigrationOperation`.

Чтобы проиллюстрировать это, давайте взглянем на реализацию операции, которая создает пользователя базы данных, используя каждый из подходов. В наших переносах мы хотим включить написание следующего кода:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>Использование Мигратионбуилдер. SQL ()

Самый простой способ реализовать пользовательскую операцию — определить метод расширения, который вызывает `MigrationBuilder.Sql()`. Ниже приведен пример, в котором создается соответствующий Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Если для миграции требуется поддержка нескольких поставщиков баз данных, можно использовать свойство `MigrationBuilder.ActiveProvider`. Ниже приведен пример, поддерживающий как Microsoft SQL Server, так и PostgreSQL.

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

Чтобы отделить пользовательскую операцию от SQL, можно определить собственный `MigrationOperation`, чтобы его представить. Затем операция передается поставщику, чтобы он мог определить соответствующий SQL.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

При таком подходе методу расширения просто требуется добавить одну из этих операций в `MigrationBuilder.Operations`.

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

Этот подход требует, чтобы каждый поставщик знал, как создать SQL для этой операции в своей службе `IMigrationsSqlGenerator`. Ниже приведен пример переопределения генератора SQL Server для управления новой операцией.

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
