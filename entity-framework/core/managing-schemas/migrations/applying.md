---
title: Применение миграций — EF Core
author: bricelam
ms.author: bricelam
ms.date: 05/06/2020
uid: core/managing-schemas/migrations/applying
ms.openlocfilehash: f83a014651fdf2262a603fb91da7984672690197
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238735"
---
# <a name="applying-migrations"></a><span data-ttu-id="2e0f4-102">Применение миграций</span><span class="sxs-lookup"><span data-stu-id="2e0f4-102">Applying Migrations</span></span>

<span data-ttu-id="2e0f4-103">После добавления миграций их необходимо развернуть и применить к базам данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-103">Once your migrations have been added, they need to be deployed and applied to your databases.</span></span> <span data-ttu-id="2e0f4-104">Для этого существуют различные стратегии, которые более подходят для рабочих сред, а другие — для жизненного цикла разработки.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-104">There are various strategies for doing this, with some being more appropriate for production environments, and others for the development lifecycle.</span></span>

> [!NOTE]
> <span data-ttu-id="2e0f4-105">Независимо от стратегии развертывания всегда следует проверять созданные миграции и тестировать их перед применением к рабочей базе данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-105">Whatever your deployment strategy, always inspect the generated migrations and test them before applying to a production database.</span></span> <span data-ttu-id="2e0f4-106">Миграция может привести к удалению столбца, когда намерение приходило переименовать его или может завершиться ошибкой по различным причинам при применении к базе данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-106">A migration may drop a column when the intent was to rename it, or may fail for various reasons when applied to a database.</span></span>

## <a name="sql-scripts"></a><span data-ttu-id="2e0f4-107">Скрипты SQL</span><span class="sxs-lookup"><span data-stu-id="2e0f4-107">SQL scripts</span></span>

<span data-ttu-id="2e0f4-108">Рекомендуемый способ развертывания миграций в рабочей базе данных — создание скриптов SQL.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-108">The recommended way to deploy migrations to a production database is by generating SQL scripts.</span></span> <span data-ttu-id="2e0f4-109">Ниже перечислены преимущества этой стратегии.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-109">The advantages of this strategy include the following:</span></span>

* <span data-ttu-id="2e0f4-110">Скрипты SQL могут быть проверены на точность; Это важно, поскольку применение изменений схемы к рабочим базам данных является потенциально опасной операцией, которая может привести к потере данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-110">SQL scripts can be reviewed for accuracy; this is important since applying schema changes to production databases is a potentially dangerous operation that could involve data loss.</span></span>
* <span data-ttu-id="2e0f4-111">В некоторых случаях сценарии могут быть настроены в соответствии с конкретными потребностями рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-111">In some cases, the scripts can be tuned to fit the specific needs of a production database.</span></span>
* <span data-ttu-id="2e0f4-112">Скрипты SQL можно использовать в сочетании с технологией развертывания и даже создавать в рамках процесса непрерывной интеграции.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-112">SQL scripts can be used in conjunction with a deployment technology, and can even be generated as part of your CI process.</span></span>
* <span data-ttu-id="2e0f4-113">Скрипты SQL могут быть предоставлены администратору баз данных и могут управляться и архивироваться отдельно.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-113">SQL scripts can be provided to a DBA, and can be managed and archived separately.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2e0f4-114">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2e0f4-114">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="2e0f4-115">Основное использование</span><span class="sxs-lookup"><span data-stu-id="2e0f4-115">Basic Usage</span></span>

<span data-ttu-id="2e0f4-116">Следующая строка создает скрипт SQL из пустой базы данных в последнюю миграцию:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-116">The following generates a SQL script from a blank database to the latest migration:</span></span>

```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="2e0f4-117">С from (в подразумеваемую миграцию)</span><span class="sxs-lookup"><span data-stu-id="2e0f4-117">With From (to implied)</span></span>

<span data-ttu-id="2e0f4-118">В следующем примере создается скрипт SQL из данной миграции в последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-118">The following generates a SQL script from the given migration to the latest migration.</span></span>

```dotnetcli
dotnet ef migrations script AddNewTables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="2e0f4-119">С from и to</span><span class="sxs-lookup"><span data-stu-id="2e0f4-119">With From and To</span></span>

<span data-ttu-id="2e0f4-120">В следующем примере создается скрипт SQL из указанной `from` миграции в указанную `to` миграцию.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-120">The following generates a SQL script from the specified `from` migration to the specified `to` migration.</span></span>

```dotnetcli
dotnet ef migrations script AddNewTables AddAuditTable
```

<span data-ttu-id="2e0f4-121">Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-121">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span>

> [!WARNING]
> <span data-ttu-id="2e0f4-122">Обязательно учитывайте возможные сценарии потери данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-122">Please take note of potential data loss scenarios.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="2e0f4-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e0f4-123">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="2e0f4-124">Основное использование</span><span class="sxs-lookup"><span data-stu-id="2e0f4-124">Basic Usage</span></span>

<span data-ttu-id="2e0f4-125">Следующая строка создает скрипт SQL из пустой базы данных в последнюю миграцию:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-125">The following generates a SQL script from a blank database to the latest migration:</span></span>

``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="2e0f4-126">С from (в подразумеваемую миграцию)</span><span class="sxs-lookup"><span data-stu-id="2e0f4-126">With From (to implied)</span></span>

<span data-ttu-id="2e0f4-127">В следующем примере создается скрипт SQL из данной миграции в последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-127">The following generates a SQL script from the given migration to the latest migration.</span></span>

```powershell
Script-Migration AddNewTables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="2e0f4-128">С from и to</span><span class="sxs-lookup"><span data-stu-id="2e0f4-128">With From and To</span></span>

<span data-ttu-id="2e0f4-129">В следующем примере создается скрипт SQL из указанной `from` миграции в указанную `to` миграцию.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-129">The following generates a SQL script from the specified `from` migration to the specified `to` migration.</span></span>

```powershell
Script-Migration AddNewTables AddAuditTable
```
<span data-ttu-id="2e0f4-130">Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-130">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="2e0f4-131">*Обязательно учитывайте возможные сценарии потери данных.*</span><span class="sxs-lookup"><span data-stu-id="2e0f4-131">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="2e0f4-132">Создание скрипта принимает следующие два аргумента в, указывая, какой диапазон миграции следует создавать:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-132">Script generation accepts the following two arguments in indicate which range of migrations should be generated:</span></span>

* <span data-ttu-id="2e0f4-133">Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-133">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="2e0f4-134">Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="2e0f4-134">If no migrations have been applied, specify `0` (this is the default).</span></span>
* <span data-ttu-id="2e0f4-135">Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-135">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="2e0f4-136">По умолчанию она является последней миграцией в проекте.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-136">This defaults to the last migration in your project.</span></span>

## <a name="idempotent-sql-scripts"></a><span data-ttu-id="2e0f4-137">Идемпотентными скрипты SQL</span><span class="sxs-lookup"><span data-stu-id="2e0f4-137">Idempotent SQL scripts</span></span>

<span data-ttu-id="2e0f4-138">Приведенные выше скрипты SQL можно применять только для изменения схемы с одной миграции на другую. Вы обязаны правильно применять сценарий и только к базе данных в правильном состоянии миграции.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-138">The SQL scripts generated above can only be applied to change your schema from one migration to another; it is your responsibility to apply the script appropriately, and only to database in the correct migration state.</span></span> <span data-ttu-id="2e0f4-139">EF Core также поддерживает создание сценариев **идемпотентными** , которые внутренне проверяют, какие миграции уже применены (с помощью таблицы журнала миграции), и применяют только недостающие.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-139">EF Core also supports generating **idempotent** scripts, which internally check which migrations have already been applied (via the migrations history table), and only apply missing ones.</span></span> <span data-ttu-id="2e0f4-140">Это полезно, если вы не точно понимаете, какая последняя миграция была применена к базе данных, или если вы развертываете несколько баз данных, которые могут быть в разных переносах.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-140">This is useful if you don't exactly know what the last migration applied to the database was, or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<span data-ttu-id="2e0f4-141">Ниже приводится пример идемпотентными миграций.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-141">The following generates idempotent migrations:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="2e0f4-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2e0f4-142">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations script --idempotent
```

#### <a name="visual-studio"></a>[<span data-ttu-id="2e0f4-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e0f4-143">Visual Studio</span></span>](#tab/vs)

``` powershell
Script-Migration -Idempotent
```

***

## <a name="command-line-tools"></a><span data-ttu-id="2e0f4-144">Программы командной строки</span><span class="sxs-lookup"><span data-stu-id="2e0f4-144">Command-line tools</span></span>

<span data-ttu-id="2e0f4-145">Программы командной строки EF можно использовать для применения миграций к базе данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-145">The EF command-line tools can be used to apply migrations to a database.</span></span> <span data-ttu-id="2e0f4-146">При работе с локальной средой разработки и тестирования миграций этот подход не идеально подходит для управления производственными базами данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-146">While productive for local development and testing of migrations, this approach isn't ideal for managing production databases:</span></span>

* <span data-ttu-id="2e0f4-147">Команды SQL применяются непосредственно средством, не предоставляя разработчику изменения для их проверки или изменения.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-147">The SQL commands are applied directly by the tool, without giving the developer a change to inspect or modify them.</span></span> <span data-ttu-id="2e0f4-148">Это может быть опасно в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-148">This can be dangerous in a production environment.</span></span>
* <span data-ttu-id="2e0f4-149">Пакет SDK для .NET и средство EF должны быть установлены на рабочих серверах.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-149">The .NET SDK and the EF tool must be installed on production servers.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2e0f4-150">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2e0f4-150">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

<span data-ttu-id="2e0f4-151">В следующем примере база данных обновляется до последней миграции:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-151">The following updates your database to the latest migration:</span></span>

```dotnetcli
dotnet ef database update
```

<span data-ttu-id="2e0f4-152">Следующий пример обновляет базу данных на заданную миграцию:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-152">The following updates your database to a given migration:</span></span>

```dotnetcli
dotnet ef database update AddNewTables
```

<span data-ttu-id="2e0f4-153">Обратите внимание, что это можно использовать для отката к предыдущей миграции.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-153">Note that this can be used to roll back to an earlier migration as well.</span></span>

> [!WARNING]
> <span data-ttu-id="2e0f4-154">Обязательно учитывайте возможные сценарии потери данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-154">Please take note of potential data loss scenarios.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="2e0f4-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e0f4-155">Visual Studio</span></span>](#tab/vs)

<span data-ttu-id="2e0f4-156">В следующем примере база данных обновляется до последней миграции:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-156">The following updates your database to the latest migration:</span></span>

``` powershell
Update-Database
```

<span data-ttu-id="2e0f4-157">Следующий пример обновляет базу данных на заданную миграцию:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-157">The following updates your database to a given migration:</span></span>

``` powershell
Update-Database AddNewTables
```

<span data-ttu-id="2e0f4-158">Обратите внимание, что это можно использовать для отката к предыдущей миграции.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-158">Note that this can be used to roll back to an earlier migration as well.</span></span>

> [!WARNING]
> <span data-ttu-id="2e0f4-159">Обязательно учитывайте возможные сценарии потери данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-159">Please take note of potential data loss scenarios.</span></span>

***

<span data-ttu-id="2e0f4-160">Дополнительные сведения о применении миграций с помощью программ командной строки см. в [справочнике по инструментам EF Core](xref:core/miscellaneous/cli/index).</span><span class="sxs-lookup"><span data-stu-id="2e0f4-160">For more information on applying migrations via the command-line tools, see the [EF Core tools reference](xref:core/miscellaneous/cli/index).</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="2e0f4-161">Применение миграции во время выполнения</span><span class="sxs-lookup"><span data-stu-id="2e0f4-161">Apply migrations at runtime</span></span>

<span data-ttu-id="2e0f4-162">Приложение может применять миграцию программным способом, как правило, во время запуска.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-162">It's possible for the application itself to apply migrations programmatically, typically during startup.</span></span> <span data-ttu-id="2e0f4-163">При работе с локальной разработкой и тестированием миграций этот подход не подходит для управления производственными базами данных по следующим причинам.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-163">While productive for local development and testing of migrations, this approach is inappropriate for managing production databases, for the following reasons:</span></span>

* <span data-ttu-id="2e0f4-164">Если выполняется несколько экземпляров приложения, то оба приложения могут попытаться одновременно применить миграцию и завершиться сбоем (или, что еще хуже, привести к повреждению данных).</span><span class="sxs-lookup"><span data-stu-id="2e0f4-164">If multiple instances of your application are running, both applications could attempt to apply the migration concurrently and fail (or worse, cause data corruption).</span></span>
* <span data-ttu-id="2e0f4-165">Аналогично, если приложение обращается к базе данных во время его миграции другим приложением, это может привести к серьезным проблемам.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-165">Similarly, if an application is accessing the database while another application migrates it, this can cause severe issues.</span></span>
* <span data-ttu-id="2e0f4-166">Приложение должно иметь повышенный уровень доступа для изменения схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-166">The application must have elevated access to modify the database schema.</span></span> <span data-ttu-id="2e0f4-167">Обычно рекомендуется ограничить разрешения базы данных приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-167">It's generally good practice to limit the application's database permissions in production.</span></span>
* <span data-ttu-id="2e0f4-168">В случае возникновения проблемы важно иметь возможность отката примененной миграции.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-168">It's important to be able to roll back an applied migration in case of an issue.</span></span> <span data-ttu-id="2e0f4-169">Другие стратегии позволяют легко и без труда использовать это окно.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-169">The other strategies provide this easily and out of the box.</span></span>
* <span data-ttu-id="2e0f4-170">Команды SQL применяются непосредственно программой, не предоставляя разработчику изменения для их проверки или изменения.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-170">The SQL commands are applied directly by the program, without giving the developer a change to inspect or modify them.</span></span> <span data-ttu-id="2e0f4-171">Это может быть опасно в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-171">This can be dangerous in a production environment.</span></span>

<span data-ttu-id="2e0f4-172">Чтобы программно применить миграции, вызовите `context.Database.Migrate()` .</span><span class="sxs-lookup"><span data-stu-id="2e0f4-172">To apply migrations programmatically, call `context.Database.Migrate()`.</span></span> <span data-ttu-id="2e0f4-173">Например, типичное приложение ASP.NET может выполнять следующие действия:</span><span class="sxs-lookup"><span data-stu-id="2e0f4-173">For example, a typical ASP.NET application can do the following:</span></span>

```c#
public static void Main(string[] args)
{
    var host = CreateHostBuilder(args).Build();

    using (var scope = host.Services.CreateScope())
    {
        var db = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
        db.Database.Migrate();
    }

    host.Run();
}
```

<span data-ttu-id="2e0f4-174">Обратите внимание, что `Migrate()` строится на основе `IMigrator` службы, которую можно использовать для более сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-174">Note that `Migrate()` builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="2e0f4-175">Для доступа к нему используйте `myDbContext.GetInfrastructure().GetService<IMigrator>()`.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-175">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

> [!WARNING]
>
> * <span data-ttu-id="2e0f4-176">Перед использованием этого подхода в рабочей среде тщательно обсудите этот подход.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-176">Carefully consider before using this approach in production.</span></span> <span data-ttu-id="2e0f4-177">Опыт показал, что простота этой стратегии развертывания определяется проблемами, которые она создает.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-177">Experience has shown that the simplicity of this deployment strategy is outweighed by the issues it creates.</span></span> <span data-ttu-id="2e0f4-178">Вместо этого рекомендуется использовать скрипты SQL.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-178">Consider using SQL scripts instead.</span></span>
> * <span data-ttu-id="2e0f4-179">Не вызывайте `EnsureCreated()` перед `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-179">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="2e0f4-180">`EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="2e0f4-180">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>
