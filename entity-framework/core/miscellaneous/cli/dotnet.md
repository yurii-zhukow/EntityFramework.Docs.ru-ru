---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754500"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="a237b-102">Средства командной строки .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="a237b-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="a237b-103">Средства командной строки .NET Entity Framework Core являются расширением кросс платформенных **dotnet** команду, которая входит в состав из [пакета SDK для .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="a237b-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="a237b-104">Если вы используете Visual Studio, мы рекомендуем [средства PMC] [ 1] , поскольку они предоставляют более интеграция.</span><span class="sxs-lookup"><span data-stu-id="a237b-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a237b-105">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="a237b-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="a237b-106">.NET Core SDK версии 2.1.300 и включает новую **dotnet ef** команды, которые совместимы с EF Core 2.0 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="a237b-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="a237b-107">Таким образом, при использовании последние версии пакета SDK .NET Core и выполнения EF Core установки не требуется и можно пропустить оставшейся части этого раздела.</span><span class="sxs-lookup"><span data-stu-id="a237b-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="a237b-108">С другой стороны **dotnet ef** средство автономной версии пакета SDK для .NET Core 2.1.300 и более поздних не совместим с EF основных версий 1.0 и 1.1.</span><span class="sxs-lookup"><span data-stu-id="a237b-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="a237b-109">Прежде чем можно работать с проекта, использующего более ранних версиях EF ядра на компьютере с .NET Core SDK 2.1.300 или более новая версия, также необходимо установить версию 2.1.200 или более ранней версии пакета SDK и настроить приложение на использование этого старую версию, изменив его  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) файла.</span><span class="sxs-lookup"><span data-stu-id="a237b-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="a237b-110">Этот файл обычно включаются в нужном каталоге решений (один выше проекта).</span><span class="sxs-lookup"><span data-stu-id="a237b-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="a237b-111">Затем можно перейти с инструкцией installlation ниже.</span><span class="sxs-lookup"><span data-stu-id="a237b-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="a237b-112">Для предыдущих версий пакета SDK для .NET Core можно установить программы командной строки .NET Core EF, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a237b-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="a237b-113">Измените файл проекта и добавьте Microsoft.EntityFrameworkCore.Tools.DotNet как элемент DotNetCliToolReference (см. ниже)</span><span class="sxs-lookup"><span data-stu-id="a237b-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="a237b-114">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a237b-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="a237b-115">Итоговый проект должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a237b-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="a237b-116">Ссылку на пакет с `PrivateAssets="All"` означает он не предоставлен в проекты, которые ссылаются на этот проект, что особенно полезно для пакетов, которые обычно используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="a237b-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="a237b-117">Если вы все выполнили правильно, необходимо успешно выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="a237b-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="a237b-118">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="a237b-118">Using the tools</span></span>
---------------
<span data-ttu-id="a237b-119">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="a237b-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a237b-120">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="a237b-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a237b-121">Целевой проект по умолчанию в проект в текущем каталоге, но можно изменить с помощью <nobr> **--проекта** </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="a237b-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="a237b-122">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="a237b-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a237b-123">Он также значения по умолчанию в проект в текущем каталоге, но можно изменить с помощью **--запускаемый проект** параметр.</span><span class="sxs-lookup"><span data-stu-id="a237b-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="a237b-124">Например, обновление базы данных в веб-приложения, который имеет ядро EF установлен в другом проекте будет выглядеть следующим образом: `dotnet ef database update --project {project-path}` (из каталога веб-приложения)</span><span class="sxs-lookup"><span data-stu-id="a237b-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="a237b-125">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="a237b-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="a237b-126">--json</span><span class="sxs-lookup"><span data-stu-id="a237b-126">--json</span></span>                           | <span data-ttu-id="a237b-127">Отображение выходных данных JSON.</span><span class="sxs-lookup"><span data-stu-id="a237b-127">Show JSON output.</span></span>           |
| <span data-ttu-id="a237b-128">-c</span><span class="sxs-lookup"><span data-stu-id="a237b-128">-c</span></span> | <span data-ttu-id="a237b-129">--контекста \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="a237b-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="a237b-130">Чтобы использовать класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="a237b-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="a237b-131">-p</span><span class="sxs-lookup"><span data-stu-id="a237b-131">-p</span></span> | <span data-ttu-id="a237b-132">--проекта \<проекта ></span><span class="sxs-lookup"><span data-stu-id="a237b-132">--project \<PROJECT></span></span>             | <span data-ttu-id="a237b-133">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="a237b-133">The project to use.</span></span>         |
| <span data-ttu-id="a237b-134">-s</span><span class="sxs-lookup"><span data-stu-id="a237b-134">-s</span></span> | <span data-ttu-id="a237b-135">--запускаемый проект \<проекта ></span><span class="sxs-lookup"><span data-stu-id="a237b-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="a237b-136">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="a237b-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="a237b-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="a237b-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="a237b-138">Требуемая версия .NET framework.</span><span class="sxs-lookup"><span data-stu-id="a237b-138">The target framework.</span></span>       |
|    | <span data-ttu-id="a237b-139">--Конфигурация \<конфигурации ></span><span class="sxs-lookup"><span data-stu-id="a237b-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="a237b-140">Используемая конфигурация.</span><span class="sxs-lookup"><span data-stu-id="a237b-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="a237b-141">Среда выполнения — \<идентификатор ></span><span class="sxs-lookup"><span data-stu-id="a237b-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="a237b-142">Использовать в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="a237b-142">The runtime to use.</span></span>         |
| <span data-ttu-id="a237b-143">-h</span><span class="sxs-lookup"><span data-stu-id="a237b-143">-h</span></span> | <span data-ttu-id="a237b-144">--Справка</span><span class="sxs-lookup"><span data-stu-id="a237b-144">--help</span></span>                           | <span data-ttu-id="a237b-145">Отобразить справку.</span><span class="sxs-lookup"><span data-stu-id="a237b-145">Show help information.</span></span>      |
| <span data-ttu-id="a237b-146">-v</span><span class="sxs-lookup"><span data-stu-id="a237b-146">-v</span></span> | <span data-ttu-id="a237b-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="a237b-147">--verbose</span></span>                        | <span data-ttu-id="a237b-148">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="a237b-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="a237b-149">--Нет цвета</span><span class="sxs-lookup"><span data-stu-id="a237b-149">--no-color</span></span>                       | <span data-ttu-id="a237b-150">Не выделения цветом выходных данных.</span><span class="sxs-lookup"><span data-stu-id="a237b-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="a237b-151">— префикс вывода</span><span class="sxs-lookup"><span data-stu-id="a237b-151">--prefix-output</span></span>                  | <span data-ttu-id="a237b-152">Префикс выходных данных с уровнем.</span><span class="sxs-lookup"><span data-stu-id="a237b-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="a237b-153">Чтобы указать среды ASP.NET Core, задайте **ASPNETCORE_ENVIRONMENT** переменной среды перед запуском.</span><span class="sxs-lookup"><span data-stu-id="a237b-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="a237b-154">Команды</span><span class="sxs-lookup"><span data-stu-id="a237b-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="a237b-155">Удаление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a237b-155">dotnet ef database drop</span></span>

<span data-ttu-id="a237b-156">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="a237b-156">Drops the database.</span></span>

<span data-ttu-id="a237b-157">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a237b-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="a237b-158">-f</span><span class="sxs-lookup"><span data-stu-id="a237b-158">-f</span></span> | <span data-ttu-id="a237b-159">--force</span><span class="sxs-lookup"><span data-stu-id="a237b-159">--force</span></span>   | <span data-ttu-id="a237b-160">Не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="a237b-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="a237b-161">— выполнение</span><span class="sxs-lookup"><span data-stu-id="a237b-161">--dry-run</span></span> | <span data-ttu-id="a237b-162">Показать базу данных, которая будет удалена, но не удалять его.</span><span class="sxs-lookup"><span data-stu-id="a237b-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="a237b-163">Обновление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a237b-163">dotnet ef database update</span></span>

<span data-ttu-id="a237b-164">Обновляет базу данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="a237b-165">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a237b-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="a237b-166">\<МИГРАЦИЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="a237b-166">\<MIGRATION></span></span> | <span data-ttu-id="a237b-167">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="a237b-167">The target migration.</span></span> <span data-ttu-id="a237b-168">Если значение равно 0, всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="a237b-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="a237b-169">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="a237b-170">сведения о dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a237b-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="a237b-171">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="a237b-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="a237b-172">Список dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a237b-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="a237b-173">Список доступных типов DbContext.</span><span class="sxs-lookup"><span data-stu-id="a237b-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="a237b-174">ef DotNet dbcontext-формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="a237b-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="a237b-175">Закрепление типы DbContext и сущности, для базы данных.</span><span class="sxs-lookup"><span data-stu-id="a237b-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a237b-176">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a237b-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="a237b-177">\<ПОДКЛЮЧЕНИЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="a237b-177">\<CONNECTION></span></span> | <span data-ttu-id="a237b-178">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a237b-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="a237b-179">\<ПОСТАВЩИК &GT;</span><span class="sxs-lookup"><span data-stu-id="a237b-179">\<PROVIDER></span></span>   | <span data-ttu-id="a237b-180">Поставщик для использования.</span><span class="sxs-lookup"><span data-stu-id="a237b-180">The provider to use.</span></span> <span data-ttu-id="a237b-181">(например, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a237b-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="a237b-182">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a237b-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a237b-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="a237b-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="a237b-184">--заметок к данным</span><span class="sxs-lookup"><span data-stu-id="a237b-184">--data-annotations</span></span>                      | <span data-ttu-id="a237b-185">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="a237b-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a237b-186">Если этот параметр опущен, используется только fluent API.</span><span class="sxs-lookup"><span data-stu-id="a237b-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a237b-187">-c</span><span class="sxs-lookup"><span data-stu-id="a237b-187">-c</span></span>              | <span data-ttu-id="a237b-188">--контекста \<имя ></span><span class="sxs-lookup"><span data-stu-id="a237b-188">--context \<NAME></span></span>                       | <span data-ttu-id="a237b-189">Имя DbContext.</span><span class="sxs-lookup"><span data-stu-id="a237b-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="a237b-190">--контекста dir \<путь ></span><span class="sxs-lookup"><span data-stu-id="a237b-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="a237b-191">Помещение DbContext файла в каталог.</span><span class="sxs-lookup"><span data-stu-id="a237b-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="a237b-192">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="a237b-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a237b-193">-f</span><span class="sxs-lookup"><span data-stu-id="a237b-193">-f</span></span>              | <span data-ttu-id="a237b-194">--force</span><span class="sxs-lookup"><span data-stu-id="a237b-194">--force</span></span>                                 | <span data-ttu-id="a237b-195">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="a237b-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="a237b-196">-o</span><span class="sxs-lookup"><span data-stu-id="a237b-196">-o</span></span>              | <span data-ttu-id="a237b-197">--выходной каталог \<путь ></span><span class="sxs-lookup"><span data-stu-id="a237b-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="a237b-198">Чтобы поместить файлы в каталог.</span><span class="sxs-lookup"><span data-stu-id="a237b-198">The directory to put files in.</span></span> <span data-ttu-id="a237b-199">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="a237b-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="a237b-200"><nobr>--схемы \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="a237b-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="a237b-201">Схемы таблиц для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="a237b-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a237b-202">-t</span><span class="sxs-lookup"><span data-stu-id="a237b-202">-t</span></span>              | <span data-ttu-id="a237b-203">--таблицы \<имя_таблицы >...</span><span class="sxs-lookup"><span data-stu-id="a237b-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="a237b-204">Таблицы для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="a237b-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="a237b-205">--Используйте имена баз данных</span><span class="sxs-lookup"><span data-stu-id="a237b-205">--use-database-names</span></span>                    | <span data-ttu-id="a237b-206">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a237b-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="a237b-207">добавить DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="a237b-207">dotnet ef migrations add</span></span>

<span data-ttu-id="a237b-208">Добавляет новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="a237b-208">Adds a new migration.</span></span>

<span data-ttu-id="a237b-209">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a237b-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="a237b-210">\<ИМЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="a237b-210">\<NAME></span></span> | <span data-ttu-id="a237b-211">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-211">The name of the migration.</span></span> |

<span data-ttu-id="a237b-212">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a237b-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a237b-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="a237b-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="a237b-214"><nobr>--выходной каталог \<путь ></nobr></span><span class="sxs-lookup"><span data-stu-id="a237b-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="a237b-215">Каталог (и пространства имен sub) для использования.</span><span class="sxs-lookup"><span data-stu-id="a237b-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a237b-216">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="a237b-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="a237b-217">Значение по умолчанию «Миграция».</span><span class="sxs-lookup"><span data-stu-id="a237b-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="a237b-218">Список миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a237b-218">dotnet ef migrations list</span></span>

<span data-ttu-id="a237b-219">Список доступных миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="a237b-220">Удалите DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="a237b-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="a237b-221">Удаляет последний переноса.</span><span class="sxs-lookup"><span data-stu-id="a237b-221">Removes the last migration.</span></span>

<span data-ttu-id="a237b-222">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a237b-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="a237b-223">-f</span><span class="sxs-lookup"><span data-stu-id="a237b-223">-f</span></span> | <span data-ttu-id="a237b-224">--force</span><span class="sxs-lookup"><span data-stu-id="a237b-224">--force</span></span> | <span data-ttu-id="a237b-225">Вернуть миграции, если он был применен к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a237b-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="a237b-226">сценарий миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a237b-226">dotnet ef migrations script</span></span>

<span data-ttu-id="a237b-227">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a237b-228">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a237b-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="a237b-229">\<ИЗ &GT;</span><span class="sxs-lookup"><span data-stu-id="a237b-229">\<FROM></span></span> | <span data-ttu-id="a237b-230">Начальный миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-230">The starting migration.</span></span> <span data-ttu-id="a237b-231">По умолчанию равно 0 (начальной базы данных).</span><span class="sxs-lookup"><span data-stu-id="a237b-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="a237b-232">\<ЧТОБЫ &GT;</span><span class="sxs-lookup"><span data-stu-id="a237b-232">\<TO></span></span>   | <span data-ttu-id="a237b-233">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-233">The ending migration.</span></span> <span data-ttu-id="a237b-234">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="a237b-235">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a237b-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a237b-236">-o</span><span class="sxs-lookup"><span data-stu-id="a237b-236">-o</span></span> | <span data-ttu-id="a237b-237">--Вывод \<файла ></span><span class="sxs-lookup"><span data-stu-id="a237b-237">--output \<FILE></span></span> | <span data-ttu-id="a237b-238">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="a237b-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="a237b-239">-i</span><span class="sxs-lookup"><span data-stu-id="a237b-239">-i</span></span> | <span data-ttu-id="a237b-240">--идемпотентными</span><span class="sxs-lookup"><span data-stu-id="a237b-240">--idempotent</span></span>     | <span data-ttu-id="a237b-241">Создание скрипта, который можно использовать для базы данных на любой миграции.</span><span class="sxs-lookup"><span data-stu-id="a237b-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
