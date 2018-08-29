---
title: .NET core CLI — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: c70003fc7e2c5381a22d26baacd3d76f32489328
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152418"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="ea2bb-102">Средства командной строки .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="ea2bb-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="ea2bb-103">Инструменты командной строки .NET Entity Framework Core представляют собой расширение для кроссплатформенного **dotnet** команду, которая входит в состав из [пакет SDK для .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="ea2bb-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="ea2bb-104">Если вы используете Visual Studio, мы рекомендуем [инструменты PMC] [ 1] , так как они предоставляют более интегрированное взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="ea2bb-105">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="ea2bb-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="ea2bb-106">На пакет SDK для .NET Core версии 2.1.300 и включает в себя более новые **dotnet ef** команд, которые совместимы с EF Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="ea2bb-107">Таким образом, если вы используете последние версии пакета SDK для .NET Core и среды выполнения EF Core, никакой установки не требуется и оставшейся части этого раздела можно проигнорировать.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="ea2bb-108">С другой стороны **dotnet ef** средство автономной версии пакета SDK для .NET Core 2.1.300 и более поздних версий не совместим с версией EF Core 1.0 и 1.1.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="ea2bb-109">Прежде чем можно работать с проектом, который использует эти более ранних версиях EF Core на компьютере с .NET Core SDK 2.1.300 или более поздняя версия, необходимо также установить версию 2.1.200 или более ранней версии пакета SDK и настроить приложение, чтобы использовать версию, изменив его  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) файла.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="ea2bb-110">Этот файл обычно включаются в каталоге решения (по одному выше проекта).</span><span class="sxs-lookup"><span data-stu-id="ea2bb-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="ea2bb-111">Затем можно перейти с инструкцией installlation ниже.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="ea2bb-112">Для предыдущих версий пакета SDK для .NET Core можно установить средства командной строки .NET Core EF, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="ea2bb-113">Измените файл проекта и добавьте Microsoft.EntityFrameworkCore.Tools.DotNet как элемент DotNetCliToolReference (см. ниже)</span><span class="sxs-lookup"><span data-stu-id="ea2bb-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="ea2bb-114">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="ea2bb-115">Итоговый проект должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="ea2bb-116">Ссылку на пакет с `PrivateAssets="All"` означает, что он не доступен для проектов, которые ссылаются на этот проект, что особенно полезно для пакетов, которые обычно используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="ea2bb-117">Если все делается вправо, вы сможете успешно, выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="ea2bb-118">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="ea2bb-118">Using the tools</span></span>
---------------
<span data-ttu-id="ea2bb-119">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="ea2bb-120">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="ea2bb-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="ea2bb-121">Проект по умолчанию проект в текущем каталоге, но можно изменить с помощью <nobr> **`--project`** </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="ea2bb-122">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="ea2bb-123">Он также по умолчанию в проект в текущем каталоге, но можно изменить с помощью **`--startup-project`** параметр.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="ea2bb-124">Например, обновление базы данных веб-приложения с EF Core установлен в виде отдельного проекта будет выглядеть следующим образом: `dotnet ef database update --project {project-path}` (из каталога веб-приложения)</span><span class="sxs-lookup"><span data-stu-id="ea2bb-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="ea2bb-125">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="ea2bb-126">Показать выходные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-126">Show JSON output.</span></span>           |
| <span data-ttu-id="ea2bb-127">-c</span><span class="sxs-lookup"><span data-stu-id="ea2bb-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="ea2bb-128">DbContext для использования.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="ea2bb-129">-p</span><span class="sxs-lookup"><span data-stu-id="ea2bb-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="ea2bb-130">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-130">The project to use.</span></span>         |
| <span data-ttu-id="ea2bb-131">-s</span><span class="sxs-lookup"><span data-stu-id="ea2bb-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="ea2bb-132">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="ea2bb-133">Целевая платформа.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="ea2bb-134">Используемая конфигурация.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="ea2bb-135">Среда выполнения для использования.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-135">The runtime to use.</span></span>         |
| <span data-ttu-id="ea2bb-136">-h</span><span class="sxs-lookup"><span data-stu-id="ea2bb-136">-h</span></span> | `--help`                           | <span data-ttu-id="ea2bb-137">Показать справочные сведения.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-137">Show help information.</span></span>      |
| <span data-ttu-id="ea2bb-138">-v</span><span class="sxs-lookup"><span data-stu-id="ea2bb-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="ea2bb-139">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="ea2bb-140">Не необходимо выделить цветом выходные данные.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="ea2bb-141">Префикс выходных данных с уровнем.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="ea2bb-142">Среда ASP.NET Core, установите **ASPNETCORE_ENVIRONMENT** переменной среды перед запуском.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="ea2bb-143">Команды</span><span class="sxs-lookup"><span data-stu-id="ea2bb-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="ea2bb-144">Удаление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ea2bb-144">dotnet ef database drop</span></span>

<span data-ttu-id="ea2bb-145">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-145">Drops the database.</span></span>

<span data-ttu-id="ea2bb-146">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="ea2bb-147">-f</span><span class="sxs-lookup"><span data-stu-id="ea2bb-147">-f</span></span> | `--force`   | <span data-ttu-id="ea2bb-148">Не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="ea2bb-149">Показать базу данных, которая будет удалена, но не удаляйте ее.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="ea2bb-150">Обновление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ea2bb-150">dotnet ef database update</span></span>

<span data-ttu-id="ea2bb-151">Обновляет базу данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="ea2bb-152">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="ea2bb-153">Целевой объект миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-153">The target migration.</span></span> <span data-ttu-id="ea2bb-154">Если значение равно 0, будут отменены все миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="ea2bb-155">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="ea2bb-156">сведения о dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ea2bb-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="ea2bb-157">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="ea2bb-158">DotNet ef dbcontext списка</span><span class="sxs-lookup"><span data-stu-id="ea2bb-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="ea2bb-159">Список доступных типов DbContext.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="ea2bb-160">DotNet ef dbcontext каркаса</span><span class="sxs-lookup"><span data-stu-id="ea2bb-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="ea2bb-161">Формирует шаблоны DbContext и типов сущностей для базы данных.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="ea2bb-162">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="ea2bb-163">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="ea2bb-164">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-164">The provider to use.</span></span> <span data-ttu-id="ea2bb-165">(например, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="ea2bb-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="ea2bb-166">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ea2bb-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="ea2bb-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="ea2bb-168">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="ea2bb-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="ea2bb-169">Если этот параметр опущен, используется только текучего API.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="ea2bb-170">-c</span><span class="sxs-lookup"><span data-stu-id="ea2bb-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="ea2bb-171">Имя DbContext.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="ea2bb-172">Поместите файл DbContext в каталог.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="ea2bb-173">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="ea2bb-174">-f</span><span class="sxs-lookup"><span data-stu-id="ea2bb-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="ea2bb-175">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="ea2bb-176">-o</span><span class="sxs-lookup"><span data-stu-id="ea2bb-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="ea2bb-177">Чтобы поместить файлы каталог.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-177">The directory to put files in.</span></span> <span data-ttu-id="ea2bb-178">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="ea2bb-179">Схемы таблиц для создания типов сущности для.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="ea2bb-180">-t</span><span class="sxs-lookup"><span data-stu-id="ea2bb-180">-t</span></span>              | <span data-ttu-id="ea2bb-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="ea2bb-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="ea2bb-182">Для создания типов сущности для таблиц.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="ea2bb-183">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="ea2bb-184">Добавление DotNet ef migrations</span><span class="sxs-lookup"><span data-stu-id="ea2bb-184">dotnet ef migrations add</span></span>

<span data-ttu-id="ea2bb-185">Добавление новой миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-185">Adds a new migration.</span></span>

<span data-ttu-id="ea2bb-186">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="ea2bb-187">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-187">The name of the migration.</span></span> |

<span data-ttu-id="ea2bb-188">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ea2bb-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="ea2bb-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="ea2bb-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="ea2bb-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="ea2bb-191">Каталог (и sub-namespace) для использования.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="ea2bb-192">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="ea2bb-193">По умолчанию используется «Миграция».</span><span class="sxs-lookup"><span data-stu-id="ea2bb-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="ea2bb-194">DotNet ef migrations списка</span><span class="sxs-lookup"><span data-stu-id="ea2bb-194">dotnet ef migrations list</span></span>

<span data-ttu-id="ea2bb-195">Список доступных миграций.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="ea2bb-196">удалить DotNet ef migrations</span><span class="sxs-lookup"><span data-stu-id="ea2bb-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="ea2bb-197">Удаление последней миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-197">Removes the last migration.</span></span>

<span data-ttu-id="ea2bb-198">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="ea2bb-199">-f</span><span class="sxs-lookup"><span data-stu-id="ea2bb-199">-f</span></span> | `--force` | <span data-ttu-id="ea2bb-200">Отменить миграцию, если она была применена к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="ea2bb-201">DotNet ef migrations сценария</span><span class="sxs-lookup"><span data-stu-id="ea2bb-201">dotnet ef migrations script</span></span>

<span data-ttu-id="ea2bb-202">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="ea2bb-203">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="ea2bb-204">Начала миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-204">The starting migration.</span></span> <span data-ttu-id="ea2bb-205">По умолчанию равно 0 (исходной базы данных).</span><span class="sxs-lookup"><span data-stu-id="ea2bb-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="ea2bb-206">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-206">The ending migration.</span></span> <span data-ttu-id="ea2bb-207">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="ea2bb-208">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ea2bb-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="ea2bb-209">-o</span><span class="sxs-lookup"><span data-stu-id="ea2bb-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="ea2bb-210">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="ea2bb-211">-i</span><span class="sxs-lookup"><span data-stu-id="ea2bb-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="ea2bb-212">Создайте скрипт, который может использоваться в любой базе данных с любой миграции.</span><span class="sxs-lookup"><span data-stu-id="ea2bb-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
