---
title: Обновление с EF Core 1,0 RC1 до RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 5300fe459ec2b8ab9bb573c7284b009249071d65
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306454"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="c6925-102">Обновление с EF Core 1,0 RC1 до 1,0 RC2</span><span class="sxs-lookup"><span data-stu-id="c6925-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="c6925-103">В этой статье приводятся рекомендации по перемещению приложения, созданного с помощью пакетов RC1, в RC2.</span><span class="sxs-lookup"><span data-stu-id="c6925-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="c6925-104">Имена и версии пакетов</span><span class="sxs-lookup"><span data-stu-id="c6925-104">Package Names and Versions</span></span>

<span data-ttu-id="c6925-105">Между RC1 и RC2 мы изменили с "Entity Framework 7" на "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="c6925-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="c6925-106">Дополнительные сведения о причинах изменений в этой записи можно узнать [по Скотт Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6925-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="c6925-107">Из-за этого изменения `EntityFramework.*` имена пакетов изменились с на `Microsoft.EntityFrameworkCore.*` и нашей версией с `7.0.0-rc1-final` на `1.0.0-rc2-final` (или `1.0.0-preview1-final` для инструментов).</span><span class="sxs-lookup"><span data-stu-id="c6925-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="c6925-108">**Необходимо полностью удалить пакеты RC1, а затем установить RC2.**</span><span class="sxs-lookup"><span data-stu-id="c6925-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="c6925-109">Ниже приведено сопоставление для некоторых распространенных пакетов.</span><span class="sxs-lookup"><span data-stu-id="c6925-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="c6925-110">Пакет RC1</span><span class="sxs-lookup"><span data-stu-id="c6925-110">RC1 Package</span></span>                                               | <span data-ttu-id="c6925-111">Эквивалент RC2</span><span class="sxs-lookup"><span data-stu-id="c6925-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="c6925-112">EntityFramework. Микрософтсклсервер 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-113">Microsoft. EntityFrameworkCore. SqlServer 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="c6925-114">EntityFramework. SQLite 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-115">Microsoft. EntityFrameworkCore. SQLite 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="c6925-116">EntityFramework7. Npgsql 3.1.0-RC1-3</span><span class="sxs-lookup"><span data-stu-id="c6925-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="c6925-117">NpgSql. EntityFrameworkCore. postgres<to be advised></span><span class="sxs-lookup"><span data-stu-id="c6925-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="c6925-118">EntityFramework. SqlServerCompact35 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-119">EntityFrameworkCore. SqlServerCompact35 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="c6925-120">EntityFramework. SqlServerCompact40 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-121">EntityFrameworkCore. SqlServerCompact40 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="c6925-122">EntityFramework. 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-123">Microsoft. EntityFrameworkCore. Memory 1.0.0 — RC2-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="c6925-124">EntityFramework. Ибмдатасервер 7.0.0 — Beta1</span><span class="sxs-lookup"><span data-stu-id="c6925-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="c6925-125">Еще не доступно для RC2</span><span class="sxs-lookup"><span data-stu-id="c6925-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="c6925-126">EntityFramework. Commands 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1:-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="c6925-128">EntityFramework. Микрософтсклсервер. Design 7.0.0 — RC1-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="c6925-129">Microsoft. EntityFrameworkCore. SqlServer. Design 1.0.0 — RC2-Final</span><span class="sxs-lookup"><span data-stu-id="c6925-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="c6925-130">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="c6925-130">Namespaces</span></span>

<span data-ttu-id="c6925-131">Вместе с именами пакетов пространства имен изменяются `Microsoft.Data.Entity.*` с `Microsoft.EntityFrameworkCore.*`на.</span><span class="sxs-lookup"><span data-stu-id="c6925-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="c6925-132">Это изменение можно выполнить с помощью поиска/замены `using Microsoft.Data.Entity` с помощью. `using Microsoft.EntityFrameworkCore`</span><span class="sxs-lookup"><span data-stu-id="c6925-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="c6925-133">Изменения в соглашении об именовании таблиц</span><span class="sxs-lookup"><span data-stu-id="c6925-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="c6925-134">Значительным функциональным изменением, которое было принято в RC2, было использование имени `DbSet<TEntity>` свойства для данной сущности в качестве имени таблицы, сопоставленной с, а не только имени класса.</span><span class="sxs-lookup"><span data-stu-id="c6925-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="c6925-135">Дополнительные сведения об этом изменении см. в описании [проблемы, связанной с объявлением](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="c6925-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="c6925-136">Для существующих приложений RC1 мы рекомендуем добавить следующий код в начало `OnModelCreating` метода для сохранения стратегии именования RC1:</span><span class="sxs-lookup"><span data-stu-id="c6925-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="c6925-137">Если вы хотите применить новую стратегию именования, мы рекомендуем успешно выполнить оставшиеся действия по обновлению, а затем удалить код и создать миграцию для применения переименований таблиц.</span><span class="sxs-lookup"><span data-stu-id="c6925-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="c6925-138">Изменения AddDbContext и Startup.cs (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c6925-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="c6925-139">В RC1 необходимо было добавить Entity Framework служб в поставщик службы приложений в `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="c6925-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="c6925-140">В RC2 можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. д.:</span><span class="sxs-lookup"><span data-stu-id="c6925-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="c6925-141">Также необходимо добавить конструктор в производный контекст, который принимает параметры контекста и передает их в базовый конструктор.</span><span class="sxs-lookup"><span data-stu-id="c6925-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="c6925-142">Это необходимо, поскольку мы удалили некоторые из самых страшного волшебства, снукк их в фоновом режиме:</span><span class="sxs-lookup"><span data-stu-id="c6925-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="c6925-143">Передача IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="c6925-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="c6925-144">Если у вас есть код RC1, передающий `IServiceProvider` в контекст, теперь он перемещается в `DbContextOptions`, а не в отдельный параметр конструктора.</span><span class="sxs-lookup"><span data-stu-id="c6925-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="c6925-145">Используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)` для задания поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="c6925-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="c6925-146">Тестирование</span><span class="sxs-lookup"><span data-stu-id="c6925-146">Testing</span></span>

<span data-ttu-id="c6925-147">Наиболее распространенным сценарием этого является управление областью базы данных в памяти при тестировании.</span><span class="sxs-lookup"><span data-stu-id="c6925-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="c6925-148">Пример этого с [тестирование](testing/index.md) помощью RC2 см. в обновленной статье о тестировании.</span><span class="sxs-lookup"><span data-stu-id="c6925-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="c6925-149">Разрешение внутренних служб от поставщика службы приложений (только ASP.NET Core проектов)</span><span class="sxs-lookup"><span data-stu-id="c6925-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="c6925-150">Если у вас есть ASP.NET Coreное приложение и требуется, чтобы EF разрешил внутренние службы от поставщика службы приложений, существует перегрузка `AddDbContext` , которая позволяет настроить следующее:</span><span class="sxs-lookup"><span data-stu-id="c6925-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="c6925-151">Рекомендуется разрешить EF управлять собственными службами, если нет причины объединить внутренние службы EF в поставщик службы приложений.</span><span class="sxs-lookup"><span data-stu-id="c6925-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="c6925-152">Основная причина, по которой это можно сделать, — использование поставщика службы приложений для замены служб, используемых EF внутри.</span><span class="sxs-lookup"><span data-stu-id="c6925-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="c6925-153">Команды DNX = > .NET CLI (только проекты ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c6925-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="c6925-154">Если вы ранее использовали `dnx ef` команды для проектов ASP.NET 5, теперь они перемещены в `dotnet ef` команды.</span><span class="sxs-lookup"><span data-stu-id="c6925-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="c6925-155">По-прежнему применяется тот же синтаксис команды.</span><span class="sxs-lookup"><span data-stu-id="c6925-155">The same command syntax still applies.</span></span> <span data-ttu-id="c6925-156">Для получения сведений `dotnet ef --help` о синтаксисе можно использовать.</span><span class="sxs-lookup"><span data-stu-id="c6925-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="c6925-157">Способ регистрации команд изменился в RC2, так как DNX заменяется интерфейсом командной строки .NET.</span><span class="sxs-lookup"><span data-stu-id="c6925-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="c6925-158">Команды теперь регистрируются в `tools` разделе в: `project.json`</span><span class="sxs-lookup"><span data-stu-id="c6925-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="c6925-159">Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для ASP.NET Core проектов (это не поддерживается в RC1).</span><span class="sxs-lookup"><span data-stu-id="c6925-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="c6925-160">Для этого по-прежнему необходимо зарегистрировать команды в `tools` `project.json` разделе.</span><span class="sxs-lookup"><span data-stu-id="c6925-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="c6925-161">Для команд диспетчера пакетов требуется PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="c6925-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="c6925-162">При использовании команд Entity Framework в консоли диспетчера пакетов в Visual Studio необходимо убедиться, что установлен PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="c6925-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="c6925-163">Это временное требование, которое будет удалено в следующем выпуске (Дополнительные сведения см. в [#5327 выпуска](https://github.com/aspnet/EntityFramework/issues/5327) ).</span><span class="sxs-lookup"><span data-stu-id="c6925-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="c6925-164">Использование "Imports" в Project. JSON</span><span class="sxs-lookup"><span data-stu-id="c6925-164">Using "imports" in project.json</span></span>

<span data-ttu-id="c6925-165">Некоторые зависимости EF Core не поддерживают .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="c6925-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="c6925-166">Для EF Core в .NET Standard и проектах .NET Core может потребоваться добавить "Imports" в Project. JSON в качестве временного решения.</span><span class="sxs-lookup"><span data-stu-id="c6925-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="c6925-167">При добавлении EF в Restore NuGet отображается следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="c6925-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="c6925-168">Чтобы устранить эту проблему, вручную импортируйте переносимый профиль "Portable-net451 + Win8".</span><span class="sxs-lookup"><span data-stu-id="c6925-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="c6925-169">Это заставляет NuGet обрабатывать эти двоичные файлы, соответствующие этой платформе, в качестве совместимой платформы с .NET Standard, даже если они не являются.</span><span class="sxs-lookup"><span data-stu-id="c6925-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="c6925-170">Несмотря на то, что "Portable-net451 + Win8 100" несовместим с .NET Standard, он достаточно совместим для перехода с PCL на .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="c6925-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="c6925-171">Импорт может быть удален, когда зависимости EF в конечном итоге обновляются до .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="c6925-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="c6925-172">В синтаксисе массива можно добавить несколько платформ в "Imports".</span><span class="sxs-lookup"><span data-stu-id="c6925-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="c6925-173">Если в проект добавляются дополнительные библиотеки, могут потребоваться другие операции импорта.</span><span class="sxs-lookup"><span data-stu-id="c6925-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="c6925-174">См. раздел [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="c6925-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
