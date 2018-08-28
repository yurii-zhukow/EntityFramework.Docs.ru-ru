---
title: .NET core CLI — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995301"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="c93d5-102">Средства командной строки .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="c93d5-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="c93d5-103">Инструменты командной строки .NET Entity Framework Core представляют собой расширение для кроссплатформенного **dotnet** команду, которая входит в состав из [пакет SDK для .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="c93d5-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="c93d5-104">Если вы используете Visual Studio, мы рекомендуем [инструменты PMC] [ 1] , так как они предоставляют более интегрированное взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="c93d5-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="c93d5-105">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="c93d5-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="c93d5-106">На пакет SDK для .NET Core версии 2.1.300 и включает в себя более новые **dotnet ef** команд, которые совместимы с EF Core 2.0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="c93d5-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="c93d5-107">Таким образом, если вы используете последние версии пакета SDK для .NET Core и среды выполнения EF Core, никакой установки не требуется и оставшейся части этого раздела можно проигнорировать.</span><span class="sxs-lookup"><span data-stu-id="c93d5-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="c93d5-108">С другой стороны **dotnet ef** средство автономной версии пакета SDK для .NET Core 2.1.300 и более поздних версий не совместим с версией EF Core 1.0 и 1.1.</span><span class="sxs-lookup"><span data-stu-id="c93d5-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="c93d5-109">Прежде чем можно работать с проектом, который использует эти более ранних версиях EF Core на компьютере с .NET Core SDK 2.1.300 или более поздняя версия, необходимо также установить версию 2.1.200 или более ранней версии пакета SDK и настроить приложение, чтобы использовать версию, изменив его  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) файла.</span><span class="sxs-lookup"><span data-stu-id="c93d5-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="c93d5-110">Этот файл обычно включаются в каталоге решения (по одному выше проекта).</span><span class="sxs-lookup"><span data-stu-id="c93d5-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="c93d5-111">Затем можно перейти с инструкцией installlation ниже.</span><span class="sxs-lookup"><span data-stu-id="c93d5-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="c93d5-112">Для предыдущих версий пакета SDK для .NET Core можно установить средства командной строки .NET Core EF, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c93d5-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="c93d5-113">Измените файл проекта и добавьте Microsoft.EntityFrameworkCore.Tools.DotNet как элемент DotNetCliToolReference (см. ниже)</span><span class="sxs-lookup"><span data-stu-id="c93d5-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="c93d5-114">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="c93d5-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="c93d5-115">Итоговый проект должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c93d5-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="c93d5-116">Ссылку на пакет с `PrivateAssets="All"` означает, что он не доступен для проектов, которые ссылаются на этот проект, что особенно полезно для пакетов, которые обычно используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="c93d5-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="c93d5-117">Если все делается вправо, вы сможете успешно, выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="c93d5-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="c93d5-118">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="c93d5-118">Using the tools</span></span>
---------------
<span data-ttu-id="c93d5-119">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="c93d5-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="c93d5-120">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="c93d5-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="c93d5-121">Проект по умолчанию проект в текущем каталоге, но можно изменить с помощью <nobr> **--проекта** </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="c93d5-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="c93d5-122">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="c93d5-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="c93d5-123">Он также по умолчанию в проект в текущем каталоге, но можно изменить с помощью **--запускаемым проектом** параметр.</span><span class="sxs-lookup"><span data-stu-id="c93d5-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="c93d5-124">Например, обновление базы данных веб-приложения с EF Core установлен в виде отдельного проекта будет выглядеть следующим образом: `dotnet ef database update --project {project-path}` (из каталога веб-приложения)</span><span class="sxs-lookup"><span data-stu-id="c93d5-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="c93d5-125">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="c93d5-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="c93d5-126">--json</span><span class="sxs-lookup"><span data-stu-id="c93d5-126">--json</span></span>                           | <span data-ttu-id="c93d5-127">Показать выходные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="c93d5-127">Show JSON output.</span></span>           |
| <span data-ttu-id="c93d5-128">-c</span><span class="sxs-lookup"><span data-stu-id="c93d5-128">-c</span></span> | <span data-ttu-id="c93d5-129">--контекста \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="c93d5-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="c93d5-130">DbContext для использования.</span><span class="sxs-lookup"><span data-stu-id="c93d5-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="c93d5-131">-p</span><span class="sxs-lookup"><span data-stu-id="c93d5-131">-p</span></span> | <span data-ttu-id="c93d5-132">--проекта \<проекта ></span><span class="sxs-lookup"><span data-stu-id="c93d5-132">--project \<PROJECT></span></span>             | <span data-ttu-id="c93d5-133">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="c93d5-133">The project to use.</span></span>         |
| <span data-ttu-id="c93d5-134">-s</span><span class="sxs-lookup"><span data-stu-id="c93d5-134">-s</span></span> | <span data-ttu-id="c93d5-135">--запускаемым проектом \<проекта ></span><span class="sxs-lookup"><span data-stu-id="c93d5-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="c93d5-136">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="c93d5-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="c93d5-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="c93d5-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="c93d5-138">Целевая платформа.</span><span class="sxs-lookup"><span data-stu-id="c93d5-138">The target framework.</span></span>       |
|    | <span data-ttu-id="c93d5-139">--Конфигурация \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="c93d5-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="c93d5-140">Используемая конфигурация.</span><span class="sxs-lookup"><span data-stu-id="c93d5-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="c93d5-141">Среда выполнения-- \<идентификатор ></span><span class="sxs-lookup"><span data-stu-id="c93d5-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="c93d5-142">Среда выполнения для использования.</span><span class="sxs-lookup"><span data-stu-id="c93d5-142">The runtime to use.</span></span>         |
| <span data-ttu-id="c93d5-143">-h</span><span class="sxs-lookup"><span data-stu-id="c93d5-143">-h</span></span> | <span data-ttu-id="c93d5-144">--справки</span><span class="sxs-lookup"><span data-stu-id="c93d5-144">--help</span></span>                           | <span data-ttu-id="c93d5-145">Показать справочные сведения.</span><span class="sxs-lookup"><span data-stu-id="c93d5-145">Show help information.</span></span>      |
| <span data-ttu-id="c93d5-146">-v</span><span class="sxs-lookup"><span data-stu-id="c93d5-146">-v</span></span> | <span data-ttu-id="c93d5-147">--verbose</span><span class="sxs-lookup"><span data-stu-id="c93d5-147">--verbose</span></span>                        | <span data-ttu-id="c93d5-148">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="c93d5-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="c93d5-149">--Нет цвета</span><span class="sxs-lookup"><span data-stu-id="c93d5-149">--no-color</span></span>                       | <span data-ttu-id="c93d5-150">Не необходимо выделить цветом выходные данные.</span><span class="sxs-lookup"><span data-stu-id="c93d5-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="c93d5-151">— префикс output</span><span class="sxs-lookup"><span data-stu-id="c93d5-151">--prefix-output</span></span>                  | <span data-ttu-id="c93d5-152">Префикс выходных данных с уровнем.</span><span class="sxs-lookup"><span data-stu-id="c93d5-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="c93d5-153">Среда ASP.NET Core, установите **ASPNETCORE_ENVIRONMENT** переменной среды перед запуском.</span><span class="sxs-lookup"><span data-stu-id="c93d5-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="c93d5-154">Команды</span><span class="sxs-lookup"><span data-stu-id="c93d5-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="c93d5-155">Удаление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="c93d5-155">dotnet ef database drop</span></span>

<span data-ttu-id="c93d5-156">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="c93d5-156">Drops the database.</span></span>

<span data-ttu-id="c93d5-157">Параметры:</span><span class="sxs-lookup"><span data-stu-id="c93d5-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="c93d5-158">-f</span><span class="sxs-lookup"><span data-stu-id="c93d5-158">-f</span></span> | <span data-ttu-id="c93d5-159">--force</span><span class="sxs-lookup"><span data-stu-id="c93d5-159">--force</span></span>   | <span data-ttu-id="c93d5-160">Не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="c93d5-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="c93d5-161">--dry-run</span><span class="sxs-lookup"><span data-stu-id="c93d5-161">--dry-run</span></span> | <span data-ttu-id="c93d5-162">Показать базу данных, которая будет удалена, но не удаляйте ее.</span><span class="sxs-lookup"><span data-stu-id="c93d5-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="c93d5-163">Обновление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="c93d5-163">dotnet ef database update</span></span>

<span data-ttu-id="c93d5-164">Обновляет базу данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="c93d5-165">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="c93d5-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="c93d5-166">\<МИГРАЦИЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="c93d5-166">\<MIGRATION></span></span> | <span data-ttu-id="c93d5-167">Целевой объект миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-167">The target migration.</span></span> <span data-ttu-id="c93d5-168">Если значение равно 0, будут отменены все миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="c93d5-169">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="c93d5-170">сведения о dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="c93d5-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="c93d5-171">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="c93d5-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="c93d5-172">DotNet ef dbcontext списка</span><span class="sxs-lookup"><span data-stu-id="c93d5-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="c93d5-173">Список доступных типов DbContext.</span><span class="sxs-lookup"><span data-stu-id="c93d5-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="c93d5-174">DotNet ef dbcontext каркаса</span><span class="sxs-lookup"><span data-stu-id="c93d5-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="c93d5-175">Формирует шаблоны DbContext и типов сущностей для базы данных.</span><span class="sxs-lookup"><span data-stu-id="c93d5-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="c93d5-176">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="c93d5-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="c93d5-177">\<ПОДКЛЮЧЕНИЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="c93d5-177">\<CONNECTION></span></span> | <span data-ttu-id="c93d5-178">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c93d5-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="c93d5-179">\<ПОСТАВЩИК &GT;</span><span class="sxs-lookup"><span data-stu-id="c93d5-179">\<PROVIDER></span></span>   | <span data-ttu-id="c93d5-180">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="c93d5-180">The provider to use.</span></span> <span data-ttu-id="c93d5-181">(например, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="c93d5-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="c93d5-182">Параметры:</span><span class="sxs-lookup"><span data-stu-id="c93d5-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c93d5-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="c93d5-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="c93d5-184">--заметок к данным</span><span class="sxs-lookup"><span data-stu-id="c93d5-184">--data-annotations</span></span>                      | <span data-ttu-id="c93d5-185">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="c93d5-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="c93d5-186">Если этот параметр опущен, используется только текучего API.</span><span class="sxs-lookup"><span data-stu-id="c93d5-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="c93d5-187">-c</span><span class="sxs-lookup"><span data-stu-id="c93d5-187">-c</span></span>              | <span data-ttu-id="c93d5-188">--контекста \<имя ></span><span class="sxs-lookup"><span data-stu-id="c93d5-188">--context \<NAME></span></span>                       | <span data-ttu-id="c93d5-189">Имя DbContext.</span><span class="sxs-lookup"><span data-stu-id="c93d5-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="c93d5-190">--контекста dir \<путь ></span><span class="sxs-lookup"><span data-stu-id="c93d5-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="c93d5-191">Поместите файл DbContext в каталог.</span><span class="sxs-lookup"><span data-stu-id="c93d5-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="c93d5-192">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="c93d5-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="c93d5-193">-f</span><span class="sxs-lookup"><span data-stu-id="c93d5-193">-f</span></span>              | <span data-ttu-id="c93d5-194">--force</span><span class="sxs-lookup"><span data-stu-id="c93d5-194">--force</span></span>                                 | <span data-ttu-id="c93d5-195">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="c93d5-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="c93d5-196">-o</span><span class="sxs-lookup"><span data-stu-id="c93d5-196">-o</span></span>              | <span data-ttu-id="c93d5-197">--выходные данные dir \<путь ></span><span class="sxs-lookup"><span data-stu-id="c93d5-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="c93d5-198">Чтобы поместить файлы каталог.</span><span class="sxs-lookup"><span data-stu-id="c93d5-198">The directory to put files in.</span></span> <span data-ttu-id="c93d5-199">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="c93d5-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="c93d5-200"><nobr>--схемы \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="c93d5-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="c93d5-201">Схемы таблиц для создания типов сущности для.</span><span class="sxs-lookup"><span data-stu-id="c93d5-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="c93d5-202">-t</span><span class="sxs-lookup"><span data-stu-id="c93d5-202">-t</span></span>              | <span data-ttu-id="c93d5-203">--таблицы \<имя_таблицы >...</span><span class="sxs-lookup"><span data-stu-id="c93d5-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="c93d5-204">Для создания типов сущности для таблиц.</span><span class="sxs-lookup"><span data-stu-id="c93d5-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="c93d5-205">--Используйте имена баз данных</span><span class="sxs-lookup"><span data-stu-id="c93d5-205">--use-database-names</span></span>                    | <span data-ttu-id="c93d5-206">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="c93d5-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="c93d5-207">Добавление DotNet ef migrations</span><span class="sxs-lookup"><span data-stu-id="c93d5-207">dotnet ef migrations add</span></span>

<span data-ttu-id="c93d5-208">Добавление новой миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-208">Adds a new migration.</span></span>

<span data-ttu-id="c93d5-209">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="c93d5-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="c93d5-210">\<ИМЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="c93d5-210">\<NAME></span></span> | <span data-ttu-id="c93d5-211">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-211">The name of the migration.</span></span> |

<span data-ttu-id="c93d5-212">Параметры:</span><span class="sxs-lookup"><span data-stu-id="c93d5-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c93d5-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="c93d5-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="c93d5-214"><nobr>--выходные данные dir \<путь ></nobr></span><span class="sxs-lookup"><span data-stu-id="c93d5-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="c93d5-215">Каталог (и sub-namespace) для использования.</span><span class="sxs-lookup"><span data-stu-id="c93d5-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="c93d5-216">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="c93d5-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="c93d5-217">По умолчанию используется «Миграция».</span><span class="sxs-lookup"><span data-stu-id="c93d5-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="c93d5-218">DotNet ef migrations списка</span><span class="sxs-lookup"><span data-stu-id="c93d5-218">dotnet ef migrations list</span></span>

<span data-ttu-id="c93d5-219">Список доступных миграций.</span><span class="sxs-lookup"><span data-stu-id="c93d5-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="c93d5-220">удалить DotNet ef migrations</span><span class="sxs-lookup"><span data-stu-id="c93d5-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="c93d5-221">Удаление последней миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-221">Removes the last migration.</span></span>

<span data-ttu-id="c93d5-222">Параметры:</span><span class="sxs-lookup"><span data-stu-id="c93d5-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="c93d5-223">-f</span><span class="sxs-lookup"><span data-stu-id="c93d5-223">-f</span></span> | <span data-ttu-id="c93d5-224">--force</span><span class="sxs-lookup"><span data-stu-id="c93d5-224">--force</span></span> | <span data-ttu-id="c93d5-225">Отменить миграцию, если она была применена к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c93d5-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="c93d5-226">DotNet ef migrations сценария</span><span class="sxs-lookup"><span data-stu-id="c93d5-226">dotnet ef migrations script</span></span>

<span data-ttu-id="c93d5-227">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="c93d5-228">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="c93d5-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="c93d5-229">\<ИЗ &GT;</span><span class="sxs-lookup"><span data-stu-id="c93d5-229">\<FROM></span></span> | <span data-ttu-id="c93d5-230">Начала миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-230">The starting migration.</span></span> <span data-ttu-id="c93d5-231">По умолчанию равно 0 (исходной базы данных).</span><span class="sxs-lookup"><span data-stu-id="c93d5-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="c93d5-232">\<ЧТОБЫ &GT;</span><span class="sxs-lookup"><span data-stu-id="c93d5-232">\<TO></span></span>   | <span data-ttu-id="c93d5-233">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-233">The ending migration.</span></span> <span data-ttu-id="c93d5-234">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="c93d5-235">Параметры:</span><span class="sxs-lookup"><span data-stu-id="c93d5-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="c93d5-236">-o</span><span class="sxs-lookup"><span data-stu-id="c93d5-236">-o</span></span> | <span data-ttu-id="c93d5-237">--выходные данные \<файла ></span><span class="sxs-lookup"><span data-stu-id="c93d5-237">--output \<FILE></span></span> | <span data-ttu-id="c93d5-238">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="c93d5-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="c93d5-239">-i</span><span class="sxs-lookup"><span data-stu-id="c93d5-239">-i</span></span> | <span data-ttu-id="c93d5-240">--идемпотентными</span><span class="sxs-lookup"><span data-stu-id="c93d5-240">--idempotent</span></span>     | <span data-ttu-id="c93d5-241">Создайте скрипт, который может использоваться в любой базе данных с любой миграции.</span><span class="sxs-lookup"><span data-stu-id="c93d5-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
