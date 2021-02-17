---
title: Применение миграций — EF Core
description: Стратегии применения миграций схемы к базам данных в рабочей среде и разработке с помощью Entity Framework Core
author: bricelam
ms.date: 05/06/2020
uid: core/managing-schemas/migrations/applying
ms.openlocfilehash: cb4e6f719ba5ab4ef70e2e1d06760db1de5658b4
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543605"
---
# <a name="applying-migrations"></a><span data-ttu-id="94183-103">Применение миграций</span><span class="sxs-lookup"><span data-stu-id="94183-103">Applying Migrations</span></span>

<span data-ttu-id="94183-104">После добавления миграций их необходимо развернуть и применить к базам данных.</span><span class="sxs-lookup"><span data-stu-id="94183-104">Once your migrations have been added, they need to be deployed and applied to your databases.</span></span> <span data-ttu-id="94183-105">Для этого существуют различные стратегии, которые более подходят для рабочих сред, а другие — для жизненного цикла разработки.</span><span class="sxs-lookup"><span data-stu-id="94183-105">There are various strategies for doing this, with some being more appropriate for production environments, and others for the development lifecycle.</span></span>

> [!NOTE]
> <span data-ttu-id="94183-106">Независимо от стратегии развертывания всегда следует проверять созданные миграции и тестировать их перед применением к рабочей базе данных.</span><span class="sxs-lookup"><span data-stu-id="94183-106">Whatever your deployment strategy, always inspect the generated migrations and test them before applying to a production database.</span></span> <span data-ttu-id="94183-107">Миграция может привести к удалению столбца, когда намерение приходило переименовать его или может завершиться ошибкой по различным причинам при применении к базе данных.</span><span class="sxs-lookup"><span data-stu-id="94183-107">A migration may drop a column when the intent was to rename it, or may fail for various reasons when applied to a database.</span></span>

## <a name="sql-scripts"></a><span data-ttu-id="94183-108">Скрипты SQL</span><span class="sxs-lookup"><span data-stu-id="94183-108">SQL scripts</span></span>

<span data-ttu-id="94183-109">Рекомендуемый способ развертывания миграций в рабочей базе данных — создание скриптов SQL.</span><span class="sxs-lookup"><span data-stu-id="94183-109">The recommended way to deploy migrations to a production database is by generating SQL scripts.</span></span> <span data-ttu-id="94183-110">Ниже перечислены преимущества этой стратегии.</span><span class="sxs-lookup"><span data-stu-id="94183-110">The advantages of this strategy include the following:</span></span>

* <span data-ttu-id="94183-111">Скрипты SQL могут быть проверены на точность; Это важно, поскольку применение изменений схемы к рабочим базам данных является потенциально опасной операцией, которая может привести к потере данных.</span><span class="sxs-lookup"><span data-stu-id="94183-111">SQL scripts can be reviewed for accuracy; this is important since applying schema changes to production databases is a potentially dangerous operation that could involve data loss.</span></span>
* <span data-ttu-id="94183-112">В некоторых случаях сценарии могут быть настроены в соответствии с конкретными потребностями рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="94183-112">In some cases, the scripts can be tuned to fit the specific needs of a production database.</span></span>
* <span data-ttu-id="94183-113">Скрипты SQL можно использовать в сочетании с технологией развертывания и даже создавать в рамках процесса непрерывной интеграции.</span><span class="sxs-lookup"><span data-stu-id="94183-113">SQL scripts can be used in conjunction with a deployment technology, and can even be generated as part of your CI process.</span></span>
* <span data-ttu-id="94183-114">Скрипты SQL могут быть предоставлены администратору баз данных и могут управляться и архивироваться отдельно.</span><span class="sxs-lookup"><span data-stu-id="94183-114">SQL scripts can be provided to a DBA, and can be managed and archived separately.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="94183-115">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="94183-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="94183-116">Основное использование</span><span class="sxs-lookup"><span data-stu-id="94183-116">Basic Usage</span></span>

<span data-ttu-id="94183-117">Следующая строка создает скрипт SQL из пустой базы данных в последнюю миграцию:</span><span class="sxs-lookup"><span data-stu-id="94183-117">The following generates a SQL script from a blank database to the latest migration:</span></span>

```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="94183-118">С from (в подразумеваемую миграцию)</span><span class="sxs-lookup"><span data-stu-id="94183-118">With From (to implied)</span></span>

<span data-ttu-id="94183-119">В следующем примере создается скрипт SQL из данной миграции в последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="94183-119">The following generates a SQL script from the given migration to the latest migration.</span></span>

```dotnetcli
dotnet ef migrations script AddNewTables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="94183-120">С from и to</span><span class="sxs-lookup"><span data-stu-id="94183-120">With From and To</span></span>

<span data-ttu-id="94183-121">В следующем примере создается скрипт SQL из указанной `from` миграции в указанную `to` миграцию.</span><span class="sxs-lookup"><span data-stu-id="94183-121">The following generates a SQL script from the specified `from` migration to the specified `to` migration.</span></span>

```dotnetcli
dotnet ef migrations script AddNewTables AddAuditTable
```

<span data-ttu-id="94183-122">Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.</span><span class="sxs-lookup"><span data-stu-id="94183-122">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span>

> [!WARNING]
> <span data-ttu-id="94183-123">Обязательно учитывайте возможные сценарии потери данных.</span><span class="sxs-lookup"><span data-stu-id="94183-123">Please take note of potential data loss scenarios.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="94183-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94183-124">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="94183-125">Основное использование</span><span class="sxs-lookup"><span data-stu-id="94183-125">Basic Usage</span></span>

<span data-ttu-id="94183-126">Следующая строка создает скрипт SQL из пустой базы данных в последнюю миграцию:</span><span class="sxs-lookup"><span data-stu-id="94183-126">The following generates a SQL script from a blank database to the latest migration:</span></span>

```powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="94183-127">С from (в подразумеваемую миграцию)</span><span class="sxs-lookup"><span data-stu-id="94183-127">With From (to implied)</span></span>

<span data-ttu-id="94183-128">В следующем примере создается скрипт SQL из данной миграции в последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="94183-128">The following generates a SQL script from the given migration to the latest migration.</span></span>

```powershell
Script-Migration AddNewTables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="94183-129">С from и to</span><span class="sxs-lookup"><span data-stu-id="94183-129">With From and To</span></span>

<span data-ttu-id="94183-130">В следующем примере создается скрипт SQL из указанной `from` миграции в указанную `to` миграцию.</span><span class="sxs-lookup"><span data-stu-id="94183-130">The following generates a SQL script from the specified `from` migration to the specified `to` migration.</span></span>

```powershell
Script-Migration AddNewTables AddAuditTable
```

<span data-ttu-id="94183-131">Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.</span><span class="sxs-lookup"><span data-stu-id="94183-131">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="94183-132">*Обязательно учитывайте возможные сценарии потери данных.*</span><span class="sxs-lookup"><span data-stu-id="94183-132">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="94183-133">Создание скрипта принимает следующие два аргумента, указывающие, какой диапазон миграций должен быть создан:</span><span class="sxs-lookup"><span data-stu-id="94183-133">Script generation accepts the following two arguments to indicate which range of migrations should be generated:</span></span>

* <span data-ttu-id="94183-134">Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта.</span><span class="sxs-lookup"><span data-stu-id="94183-134">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="94183-135">Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="94183-135">If no migrations have been applied, specify `0` (this is the default).</span></span>
* <span data-ttu-id="94183-136">Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта.</span><span class="sxs-lookup"><span data-stu-id="94183-136">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="94183-137">По умолчанию она является последней миграцией в проекте.</span><span class="sxs-lookup"><span data-stu-id="94183-137">This defaults to the last migration in your project.</span></span>

## <a name="idempotent-sql-scripts"></a><span data-ttu-id="94183-138">Идемпотентными скрипты SQL</span><span class="sxs-lookup"><span data-stu-id="94183-138">Idempotent SQL scripts</span></span>

<span data-ttu-id="94183-139">Приведенные выше скрипты SQL можно применять только для изменения схемы с одной миграции на другую. Вы обязаны правильно применять сценарий и только к базе данных в правильном состоянии миграции.</span><span class="sxs-lookup"><span data-stu-id="94183-139">The SQL scripts generated above can only be applied to change your schema from one migration to another; it is your responsibility to apply the script appropriately, and only to database in the correct migration state.</span></span> <span data-ttu-id="94183-140">EF Core также поддерживает создание сценариев **идемпотентными** , которые внутренне проверяют, какие миграции уже применены (с помощью таблицы журнала миграции), и применяют только недостающие.</span><span class="sxs-lookup"><span data-stu-id="94183-140">EF Core also supports generating **idempotent** scripts, which internally check which migrations have already been applied (via the migrations history table), and only apply missing ones.</span></span> <span data-ttu-id="94183-141">Это полезно, если вы не точно понимаете, какая последняя миграция была применена к базе данных, или если вы развертываете несколько баз данных, которые могут быть в разных переносах.</span><span class="sxs-lookup"><span data-stu-id="94183-141">This is useful if you don't exactly know what the last migration applied to the database was, or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<span data-ttu-id="94183-142">Ниже приводится пример идемпотентными миграций.</span><span class="sxs-lookup"><span data-stu-id="94183-142">The following generates idempotent migrations:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="94183-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="94183-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations script --idempotent
```

### <a name="visual-studio"></a>[<span data-ttu-id="94183-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94183-144">Visual Studio</span></span>](#tab/vs)

```powershell
Script-Migration -Idempotent
```

***

## <a name="command-line-tools"></a><span data-ttu-id="94183-145">Программы командной строки</span><span class="sxs-lookup"><span data-stu-id="94183-145">Command-line tools</span></span>

<span data-ttu-id="94183-146">Программы командной строки EF можно использовать для применения миграций к базе данных.</span><span class="sxs-lookup"><span data-stu-id="94183-146">The EF command-line tools can be used to apply migrations to a database.</span></span> <span data-ttu-id="94183-147">При работе с локальной средой разработки и тестирования миграций этот подход не идеально подходит для управления производственными базами данных.</span><span class="sxs-lookup"><span data-stu-id="94183-147">While productive for local development and testing of migrations, this approach isn't ideal for managing production databases:</span></span>

* <span data-ttu-id="94183-148">Команды SQL применяются непосредственно средством, не давая разработчику возможность их исследовать или изменять.</span><span class="sxs-lookup"><span data-stu-id="94183-148">The SQL commands are applied directly by the tool, without giving the developer a chance to inspect or modify them.</span></span> <span data-ttu-id="94183-149">Это может быть опасно в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="94183-149">This can be dangerous in a production environment.</span></span>
* <span data-ttu-id="94183-150">Пакет SDK для .NET и средство EF должны быть установлены на рабочих серверах.</span><span class="sxs-lookup"><span data-stu-id="94183-150">The .NET SDK and the EF tool must be installed on production servers.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="94183-151">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="94183-151">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

<span data-ttu-id="94183-152">В следующем примере база данных обновляется до последней миграции:</span><span class="sxs-lookup"><span data-stu-id="94183-152">The following updates your database to the latest migration:</span></span>

```dotnetcli
dotnet ef database update
```

<span data-ttu-id="94183-153">Следующий пример обновляет базу данных на заданную миграцию:</span><span class="sxs-lookup"><span data-stu-id="94183-153">The following updates your database to a given migration:</span></span>

```dotnetcli
dotnet ef database update AddNewTables
```

<span data-ttu-id="94183-154">Обратите внимание, что это можно использовать для отката к предыдущей миграции.</span><span class="sxs-lookup"><span data-stu-id="94183-154">Note that this can be used to roll back to an earlier migration as well.</span></span>

> [!WARNING]
> <span data-ttu-id="94183-155">Обязательно учитывайте возможные сценарии потери данных.</span><span class="sxs-lookup"><span data-stu-id="94183-155">Please take note of potential data loss scenarios.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="94183-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94183-156">Visual Studio</span></span>](#tab/vs)

<span data-ttu-id="94183-157">В следующем примере база данных обновляется до последней миграции:</span><span class="sxs-lookup"><span data-stu-id="94183-157">The following updates your database to the latest migration:</span></span>

```powershell
Update-Database
```

<span data-ttu-id="94183-158">Следующий пример обновляет базу данных на заданную миграцию:</span><span class="sxs-lookup"><span data-stu-id="94183-158">The following updates your database to a given migration:</span></span>

```powershell
Update-Database AddNewTables
```

<span data-ttu-id="94183-159">Обратите внимание, что это можно использовать для отката к предыдущей миграции.</span><span class="sxs-lookup"><span data-stu-id="94183-159">Note that this can be used to roll back to an earlier migration as well.</span></span>

> [!WARNING]
> <span data-ttu-id="94183-160">Обязательно учитывайте возможные сценарии потери данных.</span><span class="sxs-lookup"><span data-stu-id="94183-160">Please take note of potential data loss scenarios.</span></span>

***

<span data-ttu-id="94183-161">Дополнительные сведения о применении миграций с помощью программ командной строки см. в [справочнике по инструментам EF Core](xref:core/cli/index).</span><span class="sxs-lookup"><span data-stu-id="94183-161">For more information on applying migrations via the command-line tools, see the [EF Core tools reference](xref:core/cli/index).</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="94183-162">Применение миграции во время выполнения</span><span class="sxs-lookup"><span data-stu-id="94183-162">Apply migrations at runtime</span></span>

<span data-ttu-id="94183-163">Приложение может применять миграцию программным способом, как правило, во время запуска.</span><span class="sxs-lookup"><span data-stu-id="94183-163">It's possible for the application itself to apply migrations programmatically, typically during startup.</span></span> <span data-ttu-id="94183-164">При работе с локальной разработкой и тестированием миграций этот подход не подходит для управления производственными базами данных по следующим причинам.</span><span class="sxs-lookup"><span data-stu-id="94183-164">While productive for local development and testing of migrations, this approach is inappropriate for managing production databases, for the following reasons:</span></span>

* <span data-ttu-id="94183-165">Если выполняется несколько экземпляров приложения, то оба приложения могут попытаться одновременно применить миграцию и завершиться сбоем (или, что еще хуже, привести к повреждению данных).</span><span class="sxs-lookup"><span data-stu-id="94183-165">If multiple instances of your application are running, both applications could attempt to apply the migration concurrently and fail (or worse, cause data corruption).</span></span>
* <span data-ttu-id="94183-166">Аналогично, если приложение обращается к базе данных во время его миграции другим приложением, это может привести к серьезным проблемам.</span><span class="sxs-lookup"><span data-stu-id="94183-166">Similarly, if an application is accessing the database while another application migrates it, this can cause severe issues.</span></span>
* <span data-ttu-id="94183-167">Приложение должно иметь повышенный уровень доступа для изменения схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="94183-167">The application must have elevated access to modify the database schema.</span></span> <span data-ttu-id="94183-168">Обычно рекомендуется ограничить разрешения базы данных приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="94183-168">It's generally good practice to limit the application's database permissions in production.</span></span>
* <span data-ttu-id="94183-169">В случае возникновения проблемы важно иметь возможность отката примененной миграции.</span><span class="sxs-lookup"><span data-stu-id="94183-169">It's important to be able to roll back an applied migration in case of an issue.</span></span> <span data-ttu-id="94183-170">Другие стратегии позволяют легко и без труда использовать это окно.</span><span class="sxs-lookup"><span data-stu-id="94183-170">The other strategies provide this easily and out of the box.</span></span>
* <span data-ttu-id="94183-171">Команды SQL применяются непосредственно программой, не давая разработчику возможность их исследовать или изменять.</span><span class="sxs-lookup"><span data-stu-id="94183-171">The SQL commands are applied directly by the program, without giving the developer a chance to inspect or modify them.</span></span> <span data-ttu-id="94183-172">Это может быть опасно в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="94183-172">This can be dangerous in a production environment.</span></span>

<span data-ttu-id="94183-173">Чтобы программно применить миграции, вызовите `context.Database.Migrate()` .</span><span class="sxs-lookup"><span data-stu-id="94183-173">To apply migrations programmatically, call `context.Database.Migrate()`.</span></span> <span data-ttu-id="94183-174">Например, типичное приложение ASP.NET может выполнять следующие действия:</span><span class="sxs-lookup"><span data-stu-id="94183-174">For example, a typical ASP.NET application can do the following:</span></span>

```csharp
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

<span data-ttu-id="94183-175">Обратите внимание, что `Migrate()` строится на основе `IMigrator` службы, которую можно использовать для более сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="94183-175">Note that `Migrate()` builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="94183-176">Для доступа к нему используйте `myDbContext.GetInfrastructure().GetService<IMigrator>()`.</span><span class="sxs-lookup"><span data-stu-id="94183-176">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

> [!WARNING]
>
> * <span data-ttu-id="94183-177">Перед использованием этого подхода в рабочей среде тщательно обсудите этот подход.</span><span class="sxs-lookup"><span data-stu-id="94183-177">Carefully consider before using this approach in production.</span></span> <span data-ttu-id="94183-178">Опыт показал, что простота этой стратегии развертывания определяется проблемами, которые она создает.</span><span class="sxs-lookup"><span data-stu-id="94183-178">Experience has shown that the simplicity of this deployment strategy is outweighed by the issues it creates.</span></span> <span data-ttu-id="94183-179">Вместо этого рекомендуется создавать скрипты SQL на основе миграций.</span><span class="sxs-lookup"><span data-stu-id="94183-179">Consider generating SQL scripts from migrations instead.</span></span>
> * <span data-ttu-id="94183-180">Не вызывайте `EnsureCreated()` перед `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="94183-180">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="94183-181">`EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="94183-181">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>
