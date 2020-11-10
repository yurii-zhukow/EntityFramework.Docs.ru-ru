---
title: Миграция с несколькими поставщиками — EF Core
description: Использование миграций для управления схемами баз данных при использовании нескольких поставщиков баз данных с Entity Framework Core
author: bricelam
ms.date: 10/29/2020
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: fb8c6121a4baccf573e57b52ebeb3fcd29fe2cba
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429784"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="a31bd-103">Миграция с несколькими поставщиками</span><span class="sxs-lookup"><span data-stu-id="a31bd-103">Migrations with Multiple Providers</span></span>

<span data-ttu-id="a31bd-104">[Средства EF Core](xref:core/cli/index) только миграция шаблонов для активного поставщика.</span><span class="sxs-lookup"><span data-stu-id="a31bd-104">The [EF Core Tools](xref:core/cli/index) only scaffold migrations for the active provider.</span></span> <span data-ttu-id="a31bd-105">Однако иногда может потребоваться использовать более одного поставщика (например Microsoft SQL Server и SQLite) с DbContext.</span><span class="sxs-lookup"><span data-stu-id="a31bd-105">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="a31bd-106">Обрабатывайте это путем обслуживания нескольких наборов миграций — по одному для каждого поставщика — и добавляя миграцию на каждый из них для каждого изменения модели.</span><span class="sxs-lookup"><span data-stu-id="a31bd-106">Handle this by maintaining multiple sets of migrations--one for each provider--and adding a migration to each for every model change.</span></span>

## <a name="using-multiple-context-types"></a><span data-ttu-id="a31bd-107">Использование нескольких типов контекста</span><span class="sxs-lookup"><span data-stu-id="a31bd-107">Using multiple context types</span></span>

<span data-ttu-id="a31bd-108">Один из способов создания нескольких наборов миграции — использование одного типа DbContext для каждого поставщика.</span><span class="sxs-lookup"><span data-stu-id="a31bd-108">One way to create multiple migration sets is to use one DbContext type per provider.</span></span>

```csharp
class SqliteBlogContext : BlogContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

<span data-ttu-id="a31bd-109">Укажите тип контекста при добавлении новых миграций.</span><span class="sxs-lookup"><span data-stu-id="a31bd-109">Specify the context type when adding new migrations.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a31bd-110">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a31bd-110">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context BlogContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context SqliteBlogContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[<span data-ttu-id="a31bd-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a31bd-111">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration InitialCreate -Context BlogContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context SqliteBlogContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> <span data-ttu-id="a31bd-112">Не нужно указывать выходной каталог для последующих миграций, так как они создаются в качестве элементов того же уровня, что и последний.</span><span class="sxs-lookup"><span data-stu-id="a31bd-112">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="using-one-context-type"></a><span data-ttu-id="a31bd-113">Использование одного типа контекста</span><span class="sxs-lookup"><span data-stu-id="a31bd-113">Using one context type</span></span>

<span data-ttu-id="a31bd-114">Также можно использовать один тип DbContext.</span><span class="sxs-lookup"><span data-stu-id="a31bd-114">It's also possible to use one DbContext type.</span></span> <span data-ttu-id="a31bd-115">В настоящее время миграцию необходимо переместить в отдельную сборку.</span><span class="sxs-lookup"><span data-stu-id="a31bd-115">This currently requires moving the migrations into a separate assembly.</span></span> <span data-ttu-id="a31bd-116">Инструкции по настройке проектов см. в [разделе Использование отдельного проекта миграции](xref:core/managing-schemas/migrations/projects) .</span><span class="sxs-lookup"><span data-stu-id="a31bd-116">Please refer to [Using a Separate Migrations Project](xref:core/managing-schemas/migrations/projects) for instructions on setting up your projects.</span></span>

> [!TIP]
> <span data-ttu-id="a31bd-117">Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Schemas/TwoProjectMigrations) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="a31bd-117">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Schemas/TwoProjectMigrations).</span></span>

<span data-ttu-id="a31bd-118">Начиная с EF Core 5,0 можно передавать аргументы в приложение из средств.</span><span class="sxs-lookup"><span data-stu-id="a31bd-118">Starting in EF Core 5.0, you can pass arguments into the app from the tools.</span></span> <span data-ttu-id="a31bd-119">Это может обеспечить более рациональный рабочий процесс, который позволяет избежать внесения изменений в проект вручную при запуске инструментов.</span><span class="sxs-lookup"><span data-stu-id="a31bd-119">This can enable a more streamlined workflow that avoids having to make manual changes to the project when running the tools.</span></span>

<span data-ttu-id="a31bd-120">Ниже приведен один шаблон, который хорошо работает при использовании [универсального узла](/dotnet/core/extensions/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a31bd-120">Here's one pattern that works well when using a [Generic Host](/dotnet/core/extensions/generic-host).</span></span>

[!code-csharp[](../../../../samples/core/Schemas/TwoProjectMigrations/WorkerService1/Program.cs#snippet_CreateHostBuilder)]

<span data-ttu-id="a31bd-121">Поскольку построитель узла по умолчанию считывает конфигурацию из аргументов командной строки, можно указать поставщика при запуске средств.</span><span class="sxs-lookup"><span data-stu-id="a31bd-121">Since the default host builder reads configuration from command-line arguments, you can specify the provider when running the tools.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a31bd-122">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a31bd-122">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add MyMigration --project ../SqlServerMigrations -- --provider SqlServer
dotnet ef migrations add MyMigration --project ../SqliteMigrations -- --provider Sqlite
```

> [!TIP]
> <span data-ttu-id="a31bd-123">`--`Токен направляет `dotnet ef` все, что следует за аргументом, и не пытается проанализировать их как параметры.</span><span class="sxs-lookup"><span data-stu-id="a31bd-123">The `--` token directs `dotnet ef` to treat everything that follows as an argument and not try to parse them as options.</span></span> <span data-ttu-id="a31bd-124">Все дополнительные аргументы, не используемые, `dotnet ef` пересылаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="a31bd-124">Any extra arguments not used by `dotnet ef` are forwarded to the app.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="a31bd-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a31bd-125">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration MyMigration -Args "--provider SqlServer"
Add-Migration MyMigration -Args "--provider Sqlite"
```

***

> [!NOTE]
> <span data-ttu-id="a31bd-126">Возможность указывать дополнительные аргументы для приложения была добавлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="a31bd-126">The ability to specify additional arguments for the app was added in EF Core 5.0.</span></span> <span data-ttu-id="a31bd-127">Если вы используете более раннюю версию, укажите значения конфигурации с переменными среды.</span><span class="sxs-lookup"><span data-stu-id="a31bd-127">If you're using an older version, specify configuration values with environment variables instead.</span></span>
