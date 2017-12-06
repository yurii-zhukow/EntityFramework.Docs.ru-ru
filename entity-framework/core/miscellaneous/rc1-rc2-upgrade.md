---
title: "Обновление до версии-кандидата 2 - EF Core EF Core 1.0 RC1"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="379fe-102">Обновление до версии 1.0 RC2 EF Core 1.0 RC1</span><span class="sxs-lookup"><span data-stu-id="379fe-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="379fe-103">Эта статья содержит рекомендации по переносу приложения, построенные с помощью версии-кандидата 1 пакеты для версии-кандидата 2.</span><span class="sxs-lookup"><span data-stu-id="379fe-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="379fe-104">Имена пакетов и версий</span><span class="sxs-lookup"><span data-stu-id="379fe-104">Package Names and Versions</span></span>

<span data-ttu-id="379fe-105">Между RC1 и RC2 мы изменено с «Entity Framework 7» на «Entity Framework Core».</span><span class="sxs-lookup"><span data-stu-id="379fe-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="379fe-106">Вы можете прочитать больше о причинах изменение [эту запись Скотт Хансельман](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="379fe-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="379fe-107">Из-за этого изменения наших имена пакетов изменилось с `EntityFramework.*` для `Microsoft.EntityFrameworkCore.*` и наши версий от `7.0.0-rc1-final` для `1.0.0-rc2-final` (или `1.0.0-preview1-final` для средств).</span><span class="sxs-lookup"><span data-stu-id="379fe-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="379fe-108">**Необходимо полностью удалить пакеты версии-кандидата 1, а затем установить RC2 областей.**</span><span class="sxs-lookup"><span data-stu-id="379fe-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="379fe-109">Вот сопоставления для некоторых распространенных пакетов.</span><span class="sxs-lookup"><span data-stu-id="379fe-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="379fe-110">Пакет для версии-кандидата 1</span><span class="sxs-lookup"><span data-stu-id="379fe-110">RC1 Package</span></span>                                               | <span data-ttu-id="379fe-111">Эквивалент RC2</span><span class="sxs-lookup"><span data-stu-id="379fe-111">RC2 Equivalent</span></span>                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| <span data-ttu-id="379fe-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="379fe-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="379fe-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="379fe-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="379fe-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="379fe-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="379fe-117">NpgSql.EntityFrameworkCore.Postgres<to be advised></span><span class="sxs-lookup"><span data-stu-id="379fe-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="379fe-118">EntityFramework.SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="379fe-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="379fe-120">EntityFramework.SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-121">EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="379fe-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="379fe-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="379fe-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="379fe-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="379fe-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="379fe-125">На данный момент недоступно для RC2</span><span class="sxs-lookup"><span data-stu-id="379fe-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="379fe-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="379fe-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="379fe-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="379fe-129">Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="379fe-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="379fe-130">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="379fe-130">Namespaces</span></span>

<span data-ttu-id="379fe-131">А также имена пакетов, пространства имен изменилось с `Microsoft.Data.Entity.*` для `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="379fe-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="379fe-132">Можно обработать это изменение с поиск и замена `using Microsoft.Data.Entity` с `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="379fe-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="379fe-133">Именование изменений в таблице</span><span class="sxs-lookup"><span data-stu-id="379fe-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="379fe-134">Значительное изменение функциональных мы воспользовались в версии-кандидата 2 было принято использовать имя `DbSet<TEntity>` свойство для заданной сущности в качестве имени таблицы оно сопоставлено, а не только имя класса.</span><span class="sxs-lookup"><span data-stu-id="379fe-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="379fe-135">Можно прочитать подробнее об этом изменении в [проблему связанные объявления](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="379fe-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="379fe-136">Для существующих приложений, RC1, мы рекомендуем, добавив следующий код в начало вашей `OnModelCreating` методом обновления стратегия именования RC1:</span><span class="sxs-lookup"><span data-stu-id="379fe-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="379fe-137">Если вы хотите использовать новый стратегия именования, будет рекомендуется успешно переименовывает завершение остальные шаги обновления и затем удаления кода и создания миграции для применения в таблице.</span><span class="sxs-lookup"><span data-stu-id="379fe-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="379fe-138">AddDbContext / файле Startup.cs изменяет (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="379fe-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="379fe-139">В версии RC1, необходимо добавить службы платформы Entity Framework для поставщика службы приложений — в `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="379fe-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="379fe-140">В версии-кандидата 2, можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. п.:</span><span class="sxs-lookup"><span data-stu-id="379fe-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="379fe-141">Необходимо также добавить конструктор производного контекст, который принимает параметры контекста и передает их в конструкторе базового класса.</span><span class="sxs-lookup"><span data-stu-id="379fe-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="379fe-142">Это необходимо, поскольку мы удалили некоторые пугающие магическое значение, которое использовали их в фоновом режиме:</span><span class="sxs-lookup"><span data-stu-id="379fe-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="379fe-143">Передача в IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="379fe-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="379fe-144">При наличии версии-кандидата 1 код, который передает `IServiceProvider` в контекст это теперь перемещен `DbContextOptions`, а не параметра отдельных конструктора.</span><span class="sxs-lookup"><span data-stu-id="379fe-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="379fe-145">Используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)` для задания поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="379fe-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="379fe-146">Тестирование</span><span class="sxs-lookup"><span data-stu-id="379fe-146">Testing</span></span>

<span data-ttu-id="379fe-147">Наиболее распространенным сценарием для этого можно было управлять областью InMemory базы данных при тестировании.</span><span class="sxs-lookup"><span data-stu-id="379fe-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="379fe-148">В разделе обновленный [тестирования](testing/index.md) статьи, например, делать это с помощью версии-кандидата 2.</span><span class="sxs-lookup"><span data-stu-id="379fe-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="379fe-149">Разрешение внутренних служб от поставщика службы приложений (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="379fe-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="379fe-150">В приложении ASP.NET Core, и требуется EF для разрешения внутренних служб от поставщика службы приложения, существует ли перегрузка `AddDbContext` , можно настроить это:</span><span class="sxs-lookup"><span data-stu-id="379fe-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="379fe-151">Мы рекомендуем, позволяя EF для внутренних целей управления собственных служб, если нет причины для объединения внутренних служб EF в поставщику службы приложения.</span><span class="sxs-lookup"><span data-stu-id="379fe-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="379fe-152">Основной причиной, что хотите сделать это заключается в использовании поставщику службы приложения для замены служб, которые внутренне использует EF</span><span class="sxs-lookup"><span data-stu-id="379fe-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="379fe-153">Команды DNX = > .NET CLI (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="379fe-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="379fe-154">Если вы ранее использовали `dnx ef` команд для проектов ASP.NET 5, они теперь были перемещены в `dotnet ef` команд.</span><span class="sxs-lookup"><span data-stu-id="379fe-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="379fe-155">По-прежнему применяется тот же синтаксис команд.</span><span class="sxs-lookup"><span data-stu-id="379fe-155">The same command syntax still applies.</span></span> <span data-ttu-id="379fe-156">Можно использовать `dotnet ef --help` сведения о синтаксисе.</span><span class="sxs-lookup"><span data-stu-id="379fe-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="379fe-157">Способ регистрации команды изменилось RC2, из-за DNX, заменяемое .NET CLI.</span><span class="sxs-lookup"><span data-stu-id="379fe-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="379fe-158">Команды зарегистрированы в `tools` статьи `project.json`:</span><span class="sxs-lookup"><span data-stu-id="379fe-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="379fe-159">Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для проектов ASP.NET Core (такой синтаксис не поддерживается в версии-кандидата 1).</span><span class="sxs-lookup"><span data-stu-id="379fe-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="379fe-160">По-прежнему необходимо зарегистрировать команды в `tools` раздел `project.json` это сделать.</span><span class="sxs-lookup"><span data-stu-id="379fe-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="379fe-161">Диспетчер пакетов команд требуется PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="379fe-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="379fe-162">При использовании команды Entity Framework в консоли диспетчера пакетов в Visual Studio, затем будет необходимо убедиться, что у вас установлен PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="379fe-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="379fe-163">Это требование временный, будут удалены в следующей версии (см. [выдавать #5327](https://github.com/aspnet/EntityFramework/issues/5327) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="379fe-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="379fe-164">С помощью «imports» в project.json</span><span class="sxs-lookup"><span data-stu-id="379fe-164">Using "imports" in project.json</span></span>

<span data-ttu-id="379fe-165">Некоторые из основных EF зависимостей еще не поддерживает .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="379fe-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="379fe-166">Основные EF в проектах .NET Standard и .NET Core может потребоваться добавление «Импорт» в файл project.json в качестве временного решения.</span><span class="sxs-lookup"><span data-stu-id="379fe-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="379fe-167">При добавлении EF, восстановление NuGet будет отображаться это сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="379fe-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="379fe-168">Это решение подходит в том, чтобы вручную импортировать профиль переносимой «portable net451 + win8».</span><span class="sxs-lookup"><span data-stu-id="379fe-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="379fe-169">Это заставляет NuGet обрабатывать этот двоичные файлы, которые соответствуют это предоставить в качестве совместимая среда .NET стандарт, даже если они не являются.</span><span class="sxs-lookup"><span data-stu-id="379fe-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="379fe-170">Несмотря на то, что «portable net451 + win8» не совместима с .NET Standard 100%, он совместим достаточно для перехода со переносимой библиотеке Классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="379fe-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="379fe-171">Операции импорта можно удалить, когда зависимости EF со временем обновления до .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="379fe-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="379fe-172">Различных платформ может быть добавлен в «импорт» в синтаксис массива.</span><span class="sxs-lookup"><span data-stu-id="379fe-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="379fe-173">Другие операции импорта может оказаться необходимым, если добавить дополнительные библиотеки в проект.</span><span class="sxs-lookup"><span data-stu-id="379fe-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="379fe-174">В разделе [выдавать #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="379fe-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
