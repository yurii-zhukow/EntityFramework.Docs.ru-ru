---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="71e5a-102">Средства командной строки .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="71e5a-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="71e5a-103">Средства командной строки .NET Entity Framework Core являются расширением кросс платформенных **dotnet** команду, которая входит в состав из [пакета SDK для .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="71e5a-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="71e5a-104">Если вы используете Visual Studio, мы рекомендуем [средства PMC] [ 1] , поскольку они предоставляют более интеграция.</span><span class="sxs-lookup"><span data-stu-id="71e5a-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="71e5a-105">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="71e5a-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="71e5a-106">Установка средств командной строки .NET Core EF, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="71e5a-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="71e5a-107">Измените файл проекта и добавьте Microsoft.EntityFrameworkCore.Tools.DotNet как элемент DotNetCliToolReference (см. ниже)</span><span class="sxs-lookup"><span data-stu-id="71e5a-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="71e5a-108">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="71e5a-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="71e5a-109">Итоговый проект должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="71e5a-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="71e5a-110">Ссылку на пакет с `PrivateAssets="All"` означает он не предоставлен в проекты, которые ссылаются на этот проект, что особенно полезно для пакетов, которые обычно используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="71e5a-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="71e5a-111">Если вы все выполнили правильно, необходимо успешно выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="71e5a-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="71e5a-112">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="71e5a-112">Using the tools</span></span>
---------------
<span data-ttu-id="71e5a-113">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="71e5a-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="71e5a-114">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="71e5a-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="71e5a-115">Целевой проект по умолчанию в проект в текущем каталоге, но можно изменить с помощью <nobr> **--проекта** </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="71e5a-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="71e5a-116">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="71e5a-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="71e5a-117">Он также значения по умолчанию в проект в текущем каталоге, но можно изменить с помощью **--запускаемый проект** параметр.</span><span class="sxs-lookup"><span data-stu-id="71e5a-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="71e5a-118">Например, обновление базы данных в веб-приложения, который имеет ядро EF установлен в другом проекте будет выглядеть следующим образом: `dotnet ef database update --project {project-path}` (из каталога веб-приложения)</span><span class="sxs-lookup"><span data-stu-id="71e5a-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="71e5a-119">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="71e5a-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="71e5a-120">--json</span><span class="sxs-lookup"><span data-stu-id="71e5a-120">--json</span></span>                           | <span data-ttu-id="71e5a-121">Отображение выходных данных JSON.</span><span class="sxs-lookup"><span data-stu-id="71e5a-121">Show JSON output.</span></span>           |
| <span data-ttu-id="71e5a-122">-c</span><span class="sxs-lookup"><span data-stu-id="71e5a-122">-c</span></span> | <span data-ttu-id="71e5a-123">--контекста \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="71e5a-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="71e5a-124">Чтобы использовать класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="71e5a-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="71e5a-125">-p</span><span class="sxs-lookup"><span data-stu-id="71e5a-125">-p</span></span> | <span data-ttu-id="71e5a-126">--проекта \<проекта ></span><span class="sxs-lookup"><span data-stu-id="71e5a-126">--project \<PROJECT></span></span>             | <span data-ttu-id="71e5a-127">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="71e5a-127">The project to use.</span></span>         |
| <span data-ttu-id="71e5a-128">-s</span><span class="sxs-lookup"><span data-stu-id="71e5a-128">-s</span></span> | <span data-ttu-id="71e5a-129">--запускаемый проект \<проекта ></span><span class="sxs-lookup"><span data-stu-id="71e5a-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="71e5a-130">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="71e5a-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="71e5a-131">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="71e5a-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="71e5a-132">Требуемая версия .NET framework.</span><span class="sxs-lookup"><span data-stu-id="71e5a-132">The target framework.</span></span>       |
|    | <span data-ttu-id="71e5a-133">--Конфигурация \<конфигурации ></span><span class="sxs-lookup"><span data-stu-id="71e5a-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="71e5a-134">Используемая конфигурация.</span><span class="sxs-lookup"><span data-stu-id="71e5a-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="71e5a-135">Среда выполнения — \<идентификатор ></span><span class="sxs-lookup"><span data-stu-id="71e5a-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="71e5a-136">Использовать в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="71e5a-136">The runtime to use.</span></span>         |
| <span data-ttu-id="71e5a-137">-h</span><span class="sxs-lookup"><span data-stu-id="71e5a-137">-h</span></span> | <span data-ttu-id="71e5a-138">--Справка</span><span class="sxs-lookup"><span data-stu-id="71e5a-138">--help</span></span>                           | <span data-ttu-id="71e5a-139">Отобразить справку.</span><span class="sxs-lookup"><span data-stu-id="71e5a-139">Show help information.</span></span>      |
| <span data-ttu-id="71e5a-140">-v</span><span class="sxs-lookup"><span data-stu-id="71e5a-140">-v</span></span> | <span data-ttu-id="71e5a-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="71e5a-141">--verbose</span></span>                        | <span data-ttu-id="71e5a-142">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="71e5a-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="71e5a-143">--Нет цвета</span><span class="sxs-lookup"><span data-stu-id="71e5a-143">--no-color</span></span>                       | <span data-ttu-id="71e5a-144">Не выделения цветом выходных данных.</span><span class="sxs-lookup"><span data-stu-id="71e5a-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="71e5a-145">— префикс вывода</span><span class="sxs-lookup"><span data-stu-id="71e5a-145">--prefix-output</span></span>                  | <span data-ttu-id="71e5a-146">Префикс выходных данных с уровнем.</span><span class="sxs-lookup"><span data-stu-id="71e5a-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="71e5a-147">Чтобы указать среды ASP.NET Core, задайте **ASPNETCORE_ENVIRONMENT** переменной среды перед запуском.</span><span class="sxs-lookup"><span data-stu-id="71e5a-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="71e5a-148">Команды</span><span class="sxs-lookup"><span data-stu-id="71e5a-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="71e5a-149">Удаление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="71e5a-149">dotnet ef database drop</span></span>

<span data-ttu-id="71e5a-150">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="71e5a-150">Drops the database.</span></span>

<span data-ttu-id="71e5a-151">Параметры:</span><span class="sxs-lookup"><span data-stu-id="71e5a-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="71e5a-152">-f</span><span class="sxs-lookup"><span data-stu-id="71e5a-152">-f</span></span> | <span data-ttu-id="71e5a-153">--force</span><span class="sxs-lookup"><span data-stu-id="71e5a-153">--force</span></span>   | <span data-ttu-id="71e5a-154">Не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="71e5a-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="71e5a-155">— выполнение</span><span class="sxs-lookup"><span data-stu-id="71e5a-155">--dry-run</span></span> | <span data-ttu-id="71e5a-156">Показать базу данных, которая будет удалена, но не удалять его.</span><span class="sxs-lookup"><span data-stu-id="71e5a-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="71e5a-157">Обновление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="71e5a-157">dotnet ef database update</span></span>

<span data-ttu-id="71e5a-158">Обновляет базу данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="71e5a-159">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="71e5a-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="71e5a-160">\<МИГРАЦИЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="71e5a-160">\<MIGRATION></span></span> | <span data-ttu-id="71e5a-161">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="71e5a-161">The target migration.</span></span> <span data-ttu-id="71e5a-162">Если значение равно 0, всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="71e5a-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="71e5a-163">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="71e5a-164">сведения о dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="71e5a-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="71e5a-165">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="71e5a-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="71e5a-166">Список dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="71e5a-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="71e5a-167">Список доступных типов DbContext.</span><span class="sxs-lookup"><span data-stu-id="71e5a-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="71e5a-168">ef DotNet dbcontext-формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="71e5a-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="71e5a-169">Закрепление типы DbContext и сущности, для базы данных.</span><span class="sxs-lookup"><span data-stu-id="71e5a-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="71e5a-170">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="71e5a-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="71e5a-171">\<ПОДКЛЮЧЕНИЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="71e5a-171">\<CONNECTION></span></span> | <span data-ttu-id="71e5a-172">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="71e5a-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="71e5a-173">\<ПОСТАВЩИК &GT;</span><span class="sxs-lookup"><span data-stu-id="71e5a-173">\<PROVIDER></span></span>   | <span data-ttu-id="71e5a-174">Поставщик для использования.</span><span class="sxs-lookup"><span data-stu-id="71e5a-174">The provider to use.</span></span> <span data-ttu-id="71e5a-175">(Например:</span><span class="sxs-lookup"><span data-stu-id="71e5a-175">(E.g.</span></span> <span data-ttu-id="71e5a-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="71e5a-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="71e5a-177">Параметры:</span><span class="sxs-lookup"><span data-stu-id="71e5a-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="71e5a-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="71e5a-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="71e5a-179">--заметок к данным</span><span class="sxs-lookup"><span data-stu-id="71e5a-179">--data-annotations</span></span>                      | <span data-ttu-id="71e5a-180">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="71e5a-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="71e5a-181">Если этот параметр опущен, используется только fluent API.</span><span class="sxs-lookup"><span data-stu-id="71e5a-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="71e5a-182">-c</span><span class="sxs-lookup"><span data-stu-id="71e5a-182">-c</span></span>              | <span data-ttu-id="71e5a-183">--контекста \<имя ></span><span class="sxs-lookup"><span data-stu-id="71e5a-183">--context \<NAME></span></span>                       | <span data-ttu-id="71e5a-184">Имя DbContext.</span><span class="sxs-lookup"><span data-stu-id="71e5a-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="71e5a-185">--контекста dir \<путь ></span><span class="sxs-lookup"><span data-stu-id="71e5a-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="71e5a-186">Помещение DbContext файла в каталог.</span><span class="sxs-lookup"><span data-stu-id="71e5a-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="71e5a-187">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="71e5a-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="71e5a-188">-f</span><span class="sxs-lookup"><span data-stu-id="71e5a-188">-f</span></span>              | <span data-ttu-id="71e5a-189">--force</span><span class="sxs-lookup"><span data-stu-id="71e5a-189">--force</span></span>                                 | <span data-ttu-id="71e5a-190">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="71e5a-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="71e5a-191">-o</span><span class="sxs-lookup"><span data-stu-id="71e5a-191">-o</span></span>              | <span data-ttu-id="71e5a-192">--выходной каталог \<путь ></span><span class="sxs-lookup"><span data-stu-id="71e5a-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="71e5a-193">Чтобы поместить файлы в каталог.</span><span class="sxs-lookup"><span data-stu-id="71e5a-193">The directory to put files in.</span></span> <span data-ttu-id="71e5a-194">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="71e5a-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="71e5a-195"><nobr>--схемы \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="71e5a-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="71e5a-196">Схемы таблиц для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="71e5a-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="71e5a-197">-t</span><span class="sxs-lookup"><span data-stu-id="71e5a-197">-t</span></span>              | <span data-ttu-id="71e5a-198">--таблицы \<имя_таблицы >...</span><span class="sxs-lookup"><span data-stu-id="71e5a-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="71e5a-199">Таблицы для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="71e5a-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="71e5a-200">--Используйте имена баз данных</span><span class="sxs-lookup"><span data-stu-id="71e5a-200">--use-database-names</span></span>                    | <span data-ttu-id="71e5a-201">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="71e5a-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="71e5a-202">добавить DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="71e5a-202">dotnet ef migrations add</span></span>

<span data-ttu-id="71e5a-203">Добавляет новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="71e5a-203">Adds a new migration.</span></span>

<span data-ttu-id="71e5a-204">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="71e5a-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="71e5a-205">\<ИМЯ &GT;</span><span class="sxs-lookup"><span data-stu-id="71e5a-205">\<NAME></span></span> | <span data-ttu-id="71e5a-206">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-206">The name of the migration.</span></span> |

<span data-ttu-id="71e5a-207">Параметры:</span><span class="sxs-lookup"><span data-stu-id="71e5a-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="71e5a-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="71e5a-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="71e5a-209"><nobr>--выходной каталог \<путь ></nobr></span><span class="sxs-lookup"><span data-stu-id="71e5a-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="71e5a-210">Каталог (и пространства имен sub) для использования.</span><span class="sxs-lookup"><span data-stu-id="71e5a-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="71e5a-211">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="71e5a-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="71e5a-212">Значение по умолчанию «Миграция».</span><span class="sxs-lookup"><span data-stu-id="71e5a-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="71e5a-213">Список миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="71e5a-213">dotnet ef migrations list</span></span>

<span data-ttu-id="71e5a-214">Список доступных миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="71e5a-215">Удалите DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="71e5a-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="71e5a-216">Удаляет последний переноса.</span><span class="sxs-lookup"><span data-stu-id="71e5a-216">Removes the last migration.</span></span>

<span data-ttu-id="71e5a-217">Параметры:</span><span class="sxs-lookup"><span data-stu-id="71e5a-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="71e5a-218">-f</span><span class="sxs-lookup"><span data-stu-id="71e5a-218">-f</span></span> | <span data-ttu-id="71e5a-219">--force</span><span class="sxs-lookup"><span data-stu-id="71e5a-219">--force</span></span> | <span data-ttu-id="71e5a-220">Вернуть миграции, если он был применен к базе данных.</span><span class="sxs-lookup"><span data-stu-id="71e5a-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="71e5a-221">сценарий миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="71e5a-221">dotnet ef migrations script</span></span>

<span data-ttu-id="71e5a-222">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="71e5a-223">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="71e5a-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="71e5a-224">\<ИЗ &GT;</span><span class="sxs-lookup"><span data-stu-id="71e5a-224">\<FROM></span></span> | <span data-ttu-id="71e5a-225">Начальный миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-225">The starting migration.</span></span> <span data-ttu-id="71e5a-226">По умолчанию равно 0 (начальной базы данных).</span><span class="sxs-lookup"><span data-stu-id="71e5a-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="71e5a-227">\<ЧТОБЫ &GT;</span><span class="sxs-lookup"><span data-stu-id="71e5a-227">\<TO></span></span>   | <span data-ttu-id="71e5a-228">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-228">The ending migration.</span></span> <span data-ttu-id="71e5a-229">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="71e5a-230">Параметры:</span><span class="sxs-lookup"><span data-stu-id="71e5a-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="71e5a-231">-o</span><span class="sxs-lookup"><span data-stu-id="71e5a-231">-o</span></span> | <span data-ttu-id="71e5a-232">--Вывод \<файла ></span><span class="sxs-lookup"><span data-stu-id="71e5a-232">--output \<FILE></span></span> | <span data-ttu-id="71e5a-233">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="71e5a-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="71e5a-234">-i</span><span class="sxs-lookup"><span data-stu-id="71e5a-234">-i</span></span> | <span data-ttu-id="71e5a-235">--идемпотентными</span><span class="sxs-lookup"><span data-stu-id="71e5a-235">--idempotent</span></span>     | <span data-ttu-id="71e5a-236">Создание скрипта, который можно использовать для базы данных на любой миграции.</span><span class="sxs-lookup"><span data-stu-id="71e5a-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
