---
title: Обновление с EF Core 1,0 RC1 до RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414113"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="4a9f0-102">Обновление с EF Core 1,0 RC1 до 1,0 RC2</span><span class="sxs-lookup"><span data-stu-id="4a9f0-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="4a9f0-103">В этой статье приводятся рекомендации по перемещению приложения, созданного с помощью пакетов RC1, в RC2.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="4a9f0-104">Имена и версии пакетов</span><span class="sxs-lookup"><span data-stu-id="4a9f0-104">Package Names and Versions</span></span>

<span data-ttu-id="4a9f0-105">Между RC1 и RC2 мы изменили с "Entity Framework 7" на "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="4a9f0-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="4a9f0-106">Дополнительные сведения о причинах изменений в этой записи можно узнать [по Скотт Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="4a9f0-107">Из-за этого изменения имена пакетов изменились с `EntityFramework.*` на `Microsoft.EntityFrameworkCore.*` и наши версии с `7.0.0-rc1-final` на `1.0.0-rc2-final` (или `1.0.0-preview1-final` для средств).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="4a9f0-108">**Необходимо полностью удалить пакеты RC1, а затем установить RC2.**</span><span class="sxs-lookup"><span data-stu-id="4a9f0-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="4a9f0-109">Ниже приведено сопоставление для некоторых распространенных пакетов.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="4a9f0-110">Пакет RC1</span><span class="sxs-lookup"><span data-stu-id="4a9f0-110">RC1 Package</span></span>                                               | <span data-ttu-id="4a9f0-111">Эквивалент RC2</span><span class="sxs-lookup"><span data-stu-id="4a9f0-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="4a9f0-112">EntityFramework. Микрософтсклсервер 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-113">Microsoft. EntityFrameworkCore. SqlServer 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="4a9f0-114">EntityFramework. SQLite 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-115">Microsoft. EntityFrameworkCore. SQLite 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="4a9f0-116">EntityFramework7. Npgsql 3.1.0-RC1-3</span><span class="sxs-lookup"><span data-stu-id="4a9f0-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="4a9f0-117">NpgSql. EntityFrameworkCore. postgres <to be advised></span><span class="sxs-lookup"><span data-stu-id="4a9f0-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="4a9f0-118">EntityFramework. SqlServerCompact35 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-119">EntityFrameworkCore. SqlServerCompact35 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="4a9f0-120">EntityFramework. SqlServerCompact40 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-121">EntityFrameworkCore. SqlServerCompact40 1.0.0-RC2-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="4a9f0-122">EntityFramework. 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-123">Microsoft. EntityFrameworkCore. Memory 1.0.0 — RC2-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="4a9f0-124">EntityFramework. Ибмдатасервер 7.0.0 — Beta1</span><span class="sxs-lookup"><span data-stu-id="4a9f0-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="4a9f0-125">Еще не доступно для RC2</span><span class="sxs-lookup"><span data-stu-id="4a9f0-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="4a9f0-126">EntityFramework. Commands 7.0.0-RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1:-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="4a9f0-128">EntityFramework. Микрософтсклсервер. Design 7.0.0 — RC1-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="4a9f0-129">Microsoft. EntityFrameworkCore. SqlServer. Design 1.0.0 — RC2-Final</span><span class="sxs-lookup"><span data-stu-id="4a9f0-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="4a9f0-130">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="4a9f0-130">Namespaces</span></span>

<span data-ttu-id="4a9f0-131">Наряду с именами пакетов пространства имен изменились с `Microsoft.Data.Entity.*` на `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="4a9f0-132">Это изменение можно решить с помощью поиска и замены `using Microsoft.Data.Entity` с `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="4a9f0-133">Изменения в соглашении об именовании таблиц</span><span class="sxs-lookup"><span data-stu-id="4a9f0-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="4a9f0-134">Значительным функциональным изменением, которое мы использовали в RC2, было использование имени свойства `DbSet<TEntity>` для данной сущности в качестве имени таблицы, к которой она сопоставляется, а не просто имени класса.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="4a9f0-135">Дополнительные сведения об этом изменении см. в описании [проблемы, связанной с объявлением](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="4a9f0-136">Для существующих приложений RC1 рекомендуется добавить следующий код в начало метода `OnModelCreating` для сохранения стратегии именования RC1:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="4a9f0-137">Если вы хотите применить новую стратегию именования, мы рекомендуем успешно выполнить оставшиеся действия по обновлению, а затем удалить код и создать миграцию для применения переименований таблиц.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="4a9f0-138">Изменения AddDbContext и Startup.cs (только для проектов ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="4a9f0-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="4a9f0-139">В RC1 необходимо было добавить Entity Framework служб в `Startup.ConfigureServices(...)`поставщика службы приложений:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="4a9f0-140">В RC2 можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. д.:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="4a9f0-141">Также необходимо добавить конструктор в производный контекст, который принимает параметры контекста и передает их в базовый конструктор.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="4a9f0-142">Это необходимо, поскольку мы удалили некоторые из самых страшного волшебства, снукк их в фоновом режиме:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="4a9f0-143">Передача IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="4a9f0-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="4a9f0-144">Если у вас есть код RC1, который передает `IServiceProvider` в контекст, теперь он перемещается в `DbContextOptions`, а не в отдельный параметр конструктора.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="4a9f0-145">Для задания поставщика услуг используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)`.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="4a9f0-146">Тестирование</span><span class="sxs-lookup"><span data-stu-id="4a9f0-146">Testing</span></span>

<span data-ttu-id="4a9f0-147">Наиболее распространенным сценарием этого является управление областью базы данных в памяти при тестировании.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="4a9f0-148">Пример этого с помощью RC2 см. в обновленной статье о [тестировании](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="4a9f0-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="4a9f0-149">Разрешение внутренних служб от поставщика службы приложений (только ASP.NET Core проектов)</span><span class="sxs-lookup"><span data-stu-id="4a9f0-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="4a9f0-150">Если у вас есть ASP.NET Coreное приложение и требуется, чтобы EF разрешил внутренние службы от поставщика службы приложений, существует перегрузка `AddDbContext`, которая позволяет настроить это:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="4a9f0-151">Рекомендуется разрешить EF управлять собственными службами, если нет причины объединить внутренние службы EF в поставщик службы приложений.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="4a9f0-152">Основная причина, по которой это можно сделать, — использование поставщика службы приложений для замены служб, используемых EF внутри.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="4a9f0-153">Команды DNX = > .NET CLI (только проекты ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="4a9f0-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="4a9f0-154">Если ранее вы использовали `dnx ef` команды для проектов ASP.NET 5, теперь они перемещены в `dotnet ef` команды.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="4a9f0-155">По-прежнему применяется тот же синтаксис команды.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-155">The same command syntax still applies.</span></span> <span data-ttu-id="4a9f0-156">Для получения сведений о синтаксисе можно использовать `dotnet ef --help`.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="4a9f0-157">Способ регистрации команд изменился в RC2, так как DNX заменяется интерфейсом командной строки .NET.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="4a9f0-158">Команды теперь регистрируются в `tools` разделе `project.json`.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="4a9f0-159">Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для ASP.NET Core проектов (это не поддерживается в RC1).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="4a9f0-160">Для этого по-прежнему необходимо зарегистрировать команды в `tools` разделе `project.json`.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="4a9f0-161">Для команд диспетчера пакетов требуется PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="4a9f0-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="4a9f0-162">При использовании команд Entity Framework в консоли диспетчера пакетов в Visual Studio необходимо убедиться, что установлен PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="4a9f0-163">Это временное требование, которое будет удалено в следующем выпуске (Дополнительные сведения см. в [#5327 выпуска](https://github.com/aspnet/EntityFramework/issues/5327) ).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="4a9f0-164">Использование "Imports" в Project. JSON</span><span class="sxs-lookup"><span data-stu-id="4a9f0-164">Using "imports" in project.json</span></span>

<span data-ttu-id="4a9f0-165">Некоторые зависимости EF Core не поддерживают .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="4a9f0-166">Для EF Core в .NET Standard и проектах .NET Core может потребоваться добавить "Imports" в Project. JSON в качестве временного решения.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="4a9f0-167">При добавлении EF в Restore NuGet отображается следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="4a9f0-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="4a9f0-168">Чтобы устранить эту проблему, вручную импортируйте переносимый профиль "Portable-net451 + Win8".</span><span class="sxs-lookup"><span data-stu-id="4a9f0-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="4a9f0-169">Это заставляет NuGet обрабатывать эти двоичные файлы, соответствующие этой платформе, в качестве совместимой платформы с .NET Standard, даже если они не являются.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="4a9f0-170">Несмотря на то, что "Portable-net451 + Win8 100" несовместим с .NET Standard, он достаточно совместим для перехода с PCL на .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="4a9f0-171">Импорт может быть удален, когда зависимости EF в конечном итоге обновляются до .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="4a9f0-172">В синтаксисе массива можно добавить несколько платформ в "Imports".</span><span class="sxs-lookup"><span data-stu-id="4a9f0-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="4a9f0-173">Если в проект добавляются дополнительные библиотеки, могут потребоваться другие операции импорта.</span><span class="sxs-lookup"><span data-stu-id="4a9f0-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="4a9f0-174">См. раздел [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="4a9f0-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
