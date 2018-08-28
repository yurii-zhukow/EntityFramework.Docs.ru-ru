---
title: Обновление до версии-кандидата 2 — EF Core EF Core 1.0 RC1
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996901"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="cba80-102">Обновление с версии-кандидата 1 EF Core 1.0 до 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="cba80-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="cba80-103">Эта статья содержит руководство по переносу приложения, созданного с помощью версии-кандидата 1 пакеты для версии-кандидата 2.</span><span class="sxs-lookup"><span data-stu-id="cba80-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="cba80-104">Имена пакетов и версий</span><span class="sxs-lookup"><span data-stu-id="cba80-104">Package Names and Versions</span></span>

<span data-ttu-id="cba80-105">Между RC1 и RC2 изменено с «Entity Framework 7» на «Entity Framework Core».</span><span class="sxs-lookup"><span data-stu-id="cba80-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="cba80-106">Дополнительные сведения о причинах возникновения изменение [блога, Скотт Хансельман](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="cba80-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="cba80-107">Благодаря этому наши имена пакетов изменилось с `EntityFramework.*` для `Microsoft.EntityFrameworkCore.*` и наши версий от `7.0.0-rc1-final` для `1.0.0-rc2-final` (или `1.0.0-preview1-final` инструментов).</span><span class="sxs-lookup"><span data-stu-id="cba80-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="cba80-108">**Необходимо полностью удалить пакеты версии-кандидата 1, а затем установить RC2 объектам.**</span><span class="sxs-lookup"><span data-stu-id="cba80-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="cba80-109">Вот некоторые распространенные пакеты сопоставление.</span><span class="sxs-lookup"><span data-stu-id="cba80-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="cba80-110">Версия-кандидат 1 пакета</span><span class="sxs-lookup"><span data-stu-id="cba80-110">RC1 Package</span></span>                                               | <span data-ttu-id="cba80-111">Эквивалент RC2</span><span class="sxs-lookup"><span data-stu-id="cba80-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="cba80-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="cba80-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="cba80-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="cba80-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="cba80-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="cba80-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="cba80-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="cba80-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="cba80-118">EntityFramework.SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="cba80-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="cba80-120">EntityFramework.SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-121">EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="cba80-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="cba80-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="cba80-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="cba80-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="cba80-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="cba80-125">Еще не доступна для версии-кандидате 2</span><span class="sxs-lookup"><span data-stu-id="cba80-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="cba80-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="cba80-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="cba80-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="cba80-129">Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="cba80-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="cba80-130">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="cba80-130">Namespaces</span></span>

<span data-ttu-id="cba80-131">Вместе с именами пакетов, пространства имен изменилось с `Microsoft.Data.Entity.*` для `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="cba80-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="cba80-132">Можно обработать это изменение с помощью поиска и замены `using Microsoft.Data.Entity` с `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="cba80-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="cba80-133">Таблицы изменений в соглашениях именования</span><span class="sxs-lookup"><span data-stu-id="cba80-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="cba80-134">Значительное изменение функциональных мы воспользовались в версии-кандидата 2 мы хотели использовать имя `DbSet<TEntity>` свойство для заданной сущности в качестве имени таблицы оно сопоставлено, а не только имя класса.</span><span class="sxs-lookup"><span data-stu-id="cba80-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="cba80-135">Дополнительные сведения об этом изменении в [проблему связанные объявления](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="cba80-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="cba80-136">Для существующих приложений RC1, рекомендуется добавить следующий код в начало вашей `OnModelCreating` методом обновления стратегия именования RC1:</span><span class="sxs-lookup"><span data-stu-id="cba80-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="cba80-137">Если вы хотите внедрить новую стратегию именования, рекомендуется успешно, выполнив остальные этапы обновления и затем удаления кода и создания перехода на применение таблицы переименовывает.</span><span class="sxs-lookup"><span data-stu-id="cba80-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="cba80-138">AddDbContext / Startup.cs изменяет (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="cba80-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="cba80-139">В RC1, приходилось добавлять Entity Framework службы поставщику услуг приложения - `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="cba80-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="cba80-140">В версии-кандидата 2, можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. д.:</span><span class="sxs-lookup"><span data-stu-id="cba80-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="cba80-141">Необходимо также добавить конструктор, для вашего производного контекста, который принимает параметры контекста и передает их в базовый конструктор.</span><span class="sxs-lookup"><span data-stu-id="cba80-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="cba80-142">Это необходимо, поскольку мы удалили некоторую часть scary волшебство, использовали их в фоновом режиме:</span><span class="sxs-lookup"><span data-stu-id="cba80-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="cba80-143">Передача IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="cba80-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="cba80-144">Если у вас есть код версии-кандидата 1, который передает `IServiceProvider` к контексту, это теперь перемещена в `DbContextOptions`, а не параметра отдельных конструктора.</span><span class="sxs-lookup"><span data-stu-id="cba80-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="cba80-145">Используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)` для задания поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="cba80-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="cba80-146">Тестирование</span><span class="sxs-lookup"><span data-stu-id="cba80-146">Testing</span></span>

<span data-ttu-id="cba80-147">Наиболее распространенным сценарием для этого было управлять областью InMemory базы данных, при тестировании.</span><span class="sxs-lookup"><span data-stu-id="cba80-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="cba80-148">См. в разделе обновленный [тестирования](testing/index.md) статье пример делать это с помощью версии-кандидата 2.</span><span class="sxs-lookup"><span data-stu-id="cba80-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="cba80-149">Разрешение внутренних служб от поставщика службы приложений (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="cba80-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="cba80-150">Если у вас есть приложение ASP.NET Core и хотите, чтобы EF для разрешения внутренних служб из поставщика служб приложений, является перегрузка метода `AddDbContext` , можно настроить это:</span><span class="sxs-lookup"><span data-stu-id="cba80-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="cba80-151">Мы рекомендуем, позволяя EF для внутренних целей управления собственных служб, если нет причин для объединения внутренних служб EF в поставщику службы приложения.</span><span class="sxs-lookup"><span data-stu-id="cba80-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="cba80-152">Основная причина, что вы можете сделать это является использование поставщику службы приложений для замены службы, которые внутренне использует EF</span><span class="sxs-lookup"><span data-stu-id="cba80-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="cba80-153">Команды DNX = > .NET CLI (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="cba80-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="cba80-154">Если вы ранее использовали `dnx ef` команд для проектов ASP.NET 5, они уже перешли `dotnet ef` команды.</span><span class="sxs-lookup"><span data-stu-id="cba80-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="cba80-155">По-прежнему применяется один и тот же синтаксис команды.</span><span class="sxs-lookup"><span data-stu-id="cba80-155">The same command syntax still applies.</span></span> <span data-ttu-id="cba80-156">Можно использовать `dotnet ef --help` сведения о синтаксисе.</span><span class="sxs-lookup"><span data-stu-id="cba80-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="cba80-157">Способ регистрации команд изменилось в версии-кандидата 2, из-за DNX, заменяемое интерфейса командной строки .NET.</span><span class="sxs-lookup"><span data-stu-id="cba80-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="cba80-158">Команды зарегистрированы в `tools` статьи `project.json`:</span><span class="sxs-lookup"><span data-stu-id="cba80-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="cba80-159">Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для проектов ASP.NET Core (это не поддерживается в версии-кандидата 1).</span><span class="sxs-lookup"><span data-stu-id="cba80-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="cba80-160">По-прежнему необходимо зарегистрировать команды в `tools` раздел `project.json` это сделать.</span><span class="sxs-lookup"><span data-stu-id="cba80-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="cba80-161">Диспетчер пакетов команд требуется PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="cba80-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="cba80-162">Если вы используете команды Entity Framework в консоли диспетчера пакетов в Visual Studio, затем необходимо будет убедиться, что у вас установлен PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="cba80-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="cba80-163">Это временный требование, которое будет удален в следующем выпуске (см. в разделе [выдавать #5327](https://github.com/aspnet/EntityFramework/issues/5327) подробности).</span><span class="sxs-lookup"><span data-stu-id="cba80-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="cba80-164">С помощью «imports» в файле project.json</span><span class="sxs-lookup"><span data-stu-id="cba80-164">Using "imports" in project.json</span></span>

<span data-ttu-id="cba80-165">Некоторые из зависимостей EF Core не поддерживают .NET Standard еще.</span><span class="sxs-lookup"><span data-stu-id="cba80-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="cba80-166">EF Core в проектах .NET Standard и .NET Core может потребоваться добавление «imports» в файл project.json в качестве временного решения.</span><span class="sxs-lookup"><span data-stu-id="cba80-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="cba80-167">При добавлении EF, восстановление NuGet будет отображаться это сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="cba80-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="cba80-168">Обойти это можно вручную импортировать на переносимый профиль «portable net451 + win8».</span><span class="sxs-lookup"><span data-stu-id="cba80-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="cba80-169">Это заставляет NuGet обрабатывать это двоичные файлы, которые соответствуют это предоставить в качестве платформа, совместимая с .NET Standard, несмотря на то, что они не являются.</span><span class="sxs-lookup"><span data-stu-id="cba80-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="cba80-170">Несмотря на то, что «portable net451 + win8» не равно 100% совместим с .NET Standard, он совместим достаточно для перехода из библиотеки PCL .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="cba80-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="cba80-171">Импорты могут быть удалены, когда зависимости платформы EF в конечном счете обновление до .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="cba80-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="cba80-172">Несколько платформ могут добавляться к «imports» в синтаксис массива.</span><span class="sxs-lookup"><span data-stu-id="cba80-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="cba80-173">Других команд импорта может возникнуть необходимость в том случае, если добавить дополнительные библиотеки в проект.</span><span class="sxs-lookup"><span data-stu-id="cba80-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="cba80-174">См. в разделе [выдавать #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="cba80-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
