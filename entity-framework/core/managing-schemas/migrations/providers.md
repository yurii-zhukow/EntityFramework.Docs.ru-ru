---
title: Миграция с несколькими поставщиками — EF Core
description: Использование миграций для управления схемами баз данных при использовании нескольких поставщиков баз данных с Entity Framework Core
author: bricelam
ms.date: 10/29/2020
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: feed19abb188eebc473386b67fac62848e682d96
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024099"
---
# <a name="migrations-with-multiple-providers"></a>Миграция с несколькими поставщиками

[Средства EF Core](xref:core/cli/index) только миграция шаблонов для активного поставщика. Однако иногда может потребоваться использовать более одного поставщика (например Microsoft SQL Server и SQLite) с DbContext. Обрабатывайте это путем обслуживания нескольких наборов миграций — по одному для каждого поставщика — и добавляя миграцию на каждый из них для каждого изменения модели.

## <a name="using-multiple-context-types"></a>Использование нескольких типов контекста

Один из способов создания нескольких наборов миграции — использование одного типа DbContext для каждого поставщика.

```csharp
class SqliteBlogContext : BlogContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

Укажите тип контекста при добавлении новых миграций.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context BlogContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context SqliteBlogContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Add-Migration InitialCreate -Context BlogContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context SqliteBlogContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> Не нужно указывать выходной каталог для последующих миграций, так как они создаются в качестве элементов того же уровня, что и последний.

## <a name="using-one-context-type"></a>Использование одного типа контекста

Также можно использовать один тип DbContext. В настоящее время миграцию необходимо переместить в отдельную сборку. Инструкции по настройке проектов см. в [разделе Использование отдельного проекта миграции](xref:core/managing-schemas/migrations/projects) .

> [!TIP]
> Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Schemas/TwoProjectMigrations) из репозитория GitHub.

Начиная с EF Core 5,0 можно передавать аргументы в приложение из средств. Это может обеспечить более рациональный рабочий процесс, который позволяет избежать внесения изменений в проект вручную при запуске инструментов.

Ниже приведен один шаблон, который хорошо работает при использовании [универсального узла](/dotnet/core/extensions/generic-host).

[!code-csharp[](../../../../samples/core/Schemas/TwoProjectMigrations/WorkerService1/Program.cs#snippet_CreateHostBuilder)]

Поскольку построитель узла по умолчанию считывает конфигурацию из аргументов командной строки, можно указать поставщика при запуске средств.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add MyMigration --project ../SqlServerMigrations -- --provider SqlServer
dotnet ef migrations add MyMigration --project ../SqliteMigrations -- --provider Sqlite
```

> [!TIP]
> `--`Токен направляет `dotnet ef` все, что следует за аргументом, и не пытается проанализировать их как параметры. Все дополнительные аргументы, не используемые, `dotnet ef` пересылаются в приложение.

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Add-Migration MyMigration -Args "--provider SqlServer"
Add-Migration MyMigration -Args "--provider Sqlite"
```

***

> [!NOTE]
> Возможность указывать дополнительные аргументы для приложения была добавлена в EF Core 5,0. Если вы используете более раннюю версию, укажите значения конфигурации с переменными среды.
