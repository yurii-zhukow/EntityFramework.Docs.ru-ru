---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="ee213-102">Средства командной строки .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="ee213-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="ee213-103">Средства командной строки .NET Entity Framework Core являются расширением кросс платформенных **dotnet** команду, которая входит в состав из [пакета SDK для .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="ee213-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="ee213-104">Если вы используете Visual Studio, мы рекомендуем [средства PMC] [ 1] , поскольку они предоставляют более интеграция.</span><span class="sxs-lookup"><span data-stu-id="ee213-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="ee213-105">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="ee213-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="ee213-106">Установка средств командной строки .NET Core EF, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ee213-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="ee213-107">Измените файл проекта и добавьте Microsoft.EntityFrameworkCore.Tools.DotNet как элемент DotNetCliToolReference (см. ниже)</span><span class="sxs-lookup"><span data-stu-id="ee213-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="ee213-108">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="ee213-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="ee213-109">Итоговый проект должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ee213-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="ee213-110">Ссылку на пакет с `PrivateAssets="All"` означает он не предоставлен в проекты, которые ссылаются на этот проект, что особенно полезно для пакетов, которые обычно используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="ee213-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="ee213-111">Если вы все выполнили правильно, необходимо успешно выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="ee213-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="ee213-112">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="ee213-112">Using the tools</span></span>
---------------
<span data-ttu-id="ee213-113">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="ee213-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="ee213-114">Целевой проект — там, где добавляются все файлы (или в некоторых случаях удалить).</span><span class="sxs-lookup"><span data-stu-id="ee213-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="ee213-115">Целевой проект по умолчанию в проект в текущем каталоге, но можно изменить с помощью <nobr> **--проекта** </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="ee213-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="ee213-116">Запускаемый проект является эмулировать средствами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="ee213-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="ee213-117">Он также значения по умолчанию в проект в текущем каталоге, но можно изменить с помощью **--запускаемый проект** параметр.</span><span class="sxs-lookup"><span data-stu-id="ee213-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="ee213-118">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="ee213-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="ee213-119">--json</span><span class="sxs-lookup"><span data-stu-id="ee213-119">--json</span></span>                           | <span data-ttu-id="ee213-120">Отображение выходных данных JSON.</span><span class="sxs-lookup"><span data-stu-id="ee213-120">Show JSON output.</span></span>           |
| <span data-ttu-id="ee213-121">-c</span><span class="sxs-lookup"><span data-stu-id="ee213-121">-c</span></span> | <span data-ttu-id="ee213-122">--контекста \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="ee213-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="ee213-123">Чтобы использовать класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="ee213-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="ee213-124">-P</span><span class="sxs-lookup"><span data-stu-id="ee213-124">-p</span></span> | <span data-ttu-id="ee213-125">--проекта \<проекта ></span><span class="sxs-lookup"><span data-stu-id="ee213-125">--project \<PROJECT></span></span>             | <span data-ttu-id="ee213-126">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="ee213-126">The project to use.</span></span>         |
| <span data-ttu-id="ee213-127">-s</span><span class="sxs-lookup"><span data-stu-id="ee213-127">-s</span></span> | <span data-ttu-id="ee213-128">--запускаемый проект \<проекта ></span><span class="sxs-lookup"><span data-stu-id="ee213-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="ee213-129">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="ee213-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="ee213-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="ee213-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="ee213-131">Требуемая версия .NET framework.</span><span class="sxs-lookup"><span data-stu-id="ee213-131">The target framework.</span></span>       |
|    | <span data-ttu-id="ee213-132">--Конфигурация \<конфигурации ></span><span class="sxs-lookup"><span data-stu-id="ee213-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="ee213-133">Используемая конфигурация.</span><span class="sxs-lookup"><span data-stu-id="ee213-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="ee213-134">Среда выполнения — \<идентификатор ></span><span class="sxs-lookup"><span data-stu-id="ee213-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="ee213-135">Использовать в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="ee213-135">The runtime to use.</span></span>         |
| <span data-ttu-id="ee213-136">-h</span><span class="sxs-lookup"><span data-stu-id="ee213-136">-h</span></span> | <span data-ttu-id="ee213-137">--Справка</span><span class="sxs-lookup"><span data-stu-id="ee213-137">--help</span></span>                           | <span data-ttu-id="ee213-138">Отобразить справку.</span><span class="sxs-lookup"><span data-stu-id="ee213-138">Show help information.</span></span>      |
| <span data-ttu-id="ee213-139">-v</span><span class="sxs-lookup"><span data-stu-id="ee213-139">-v</span></span> | <span data-ttu-id="ee213-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="ee213-140">--verbose</span></span>                        | <span data-ttu-id="ee213-141">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="ee213-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="ee213-142">--Нет цвета</span><span class="sxs-lookup"><span data-stu-id="ee213-142">--no-color</span></span>                       | <span data-ttu-id="ee213-143">Не выделения цветом выходных данных.</span><span class="sxs-lookup"><span data-stu-id="ee213-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="ee213-144">— префикс вывода</span><span class="sxs-lookup"><span data-stu-id="ee213-144">--prefix-output</span></span>                  | <span data-ttu-id="ee213-145">Префикс выходных данных с уровнем.</span><span class="sxs-lookup"><span data-stu-id="ee213-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="ee213-146">Чтобы указать среды ASP.NET Core, задайте **ASPNETCORE_ENVIRONMENT** переменной среды перед запуском.</span><span class="sxs-lookup"><span data-stu-id="ee213-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="ee213-147">Команды</span><span class="sxs-lookup"><span data-stu-id="ee213-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="ee213-148">Удаление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ee213-148">dotnet ef database drop</span></span>

<span data-ttu-id="ee213-149">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="ee213-149">Drops the database.</span></span>

<span data-ttu-id="ee213-150">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ee213-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="ee213-151">-f</span><span class="sxs-lookup"><span data-stu-id="ee213-151">-f</span></span> | <span data-ttu-id="ee213-152">--force</span><span class="sxs-lookup"><span data-stu-id="ee213-152">--force</span></span>   | <span data-ttu-id="ee213-153">Не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="ee213-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="ee213-154">— выполнение</span><span class="sxs-lookup"><span data-stu-id="ee213-154">--dry-run</span></span> | <span data-ttu-id="ee213-155">Показать базу данных, которая будет удалена, но не удалять его.</span><span class="sxs-lookup"><span data-stu-id="ee213-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="ee213-156">Обновление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ee213-156">dotnet ef database update</span></span>

<span data-ttu-id="ee213-157">Обновляет базу данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="ee213-158">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ee213-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="ee213-159">\<МИГРАЦИЯ ></span><span class="sxs-lookup"><span data-stu-id="ee213-159">\<MIGRATION></span></span> | <span data-ttu-id="ee213-160">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="ee213-160">The target migration.</span></span> <span data-ttu-id="ee213-161">Если значение равно 0, всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="ee213-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="ee213-162">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="ee213-163">сведения о dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ee213-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="ee213-164">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="ee213-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="ee213-165">Список dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ee213-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="ee213-166">Список доступных типов DbContext.</span><span class="sxs-lookup"><span data-stu-id="ee213-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="ee213-167">ef DotNet dbcontext-формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="ee213-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="ee213-168">Закрепление типы DbContext и сущности, для базы данных.</span><span class="sxs-lookup"><span data-stu-id="ee213-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="ee213-169">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ee213-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="ee213-170">\<ПОДКЛЮЧЕНИЯ ></span><span class="sxs-lookup"><span data-stu-id="ee213-170">\<CONNECTION></span></span> | <span data-ttu-id="ee213-171">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ee213-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="ee213-172">\<ПОСТАВЩИК ></span><span class="sxs-lookup"><span data-stu-id="ee213-172">\<PROVIDER></span></span>   | <span data-ttu-id="ee213-173">Поставщик для использования.</span><span class="sxs-lookup"><span data-stu-id="ee213-173">The provider to use.</span></span> <span data-ttu-id="ee213-174">(Пример</span><span class="sxs-lookup"><span data-stu-id="ee213-174">(E.g.</span></span> <span data-ttu-id="ee213-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="ee213-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="ee213-176">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ee213-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="ee213-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="ee213-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="ee213-178">--заметок к данным</span><span class="sxs-lookup"><span data-stu-id="ee213-178">--data-annotations</span></span>                | <span data-ttu-id="ee213-179">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="ee213-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="ee213-180">Если этот параметр опущен, используется только fluent API.</span><span class="sxs-lookup"><span data-stu-id="ee213-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="ee213-181">-c</span><span class="sxs-lookup"><span data-stu-id="ee213-181">-c</span></span>        |       <span data-ttu-id="ee213-182">--контекста \<имя ></span><span class="sxs-lookup"><span data-stu-id="ee213-182">--context \<NAME></span></span>                 | <span data-ttu-id="ee213-183">Имя DbContext.</span><span class="sxs-lookup"><span data-stu-id="ee213-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="ee213-184">-f</span><span class="sxs-lookup"><span data-stu-id="ee213-184">-f</span></span>        |       <span data-ttu-id="ee213-185">--force</span><span class="sxs-lookup"><span data-stu-id="ee213-185">--force</span></span>                           | <span data-ttu-id="ee213-186">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="ee213-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="ee213-187">-o</span><span class="sxs-lookup"><span data-stu-id="ee213-187">-o</span></span>        |       <span data-ttu-id="ee213-188">--выходной каталог \<путь ></span><span class="sxs-lookup"><span data-stu-id="ee213-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="ee213-189">Чтобы поместить файлы в каталог.</span><span class="sxs-lookup"><span data-stu-id="ee213-189">The directory to put files in.</span></span> <span data-ttu-id="ee213-190">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="ee213-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="ee213-191"><nobr>--схемы \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="ee213-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="ee213-192">Схемы таблиц для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="ee213-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="ee213-193">-t</span><span class="sxs-lookup"><span data-stu-id="ee213-193">-t</span></span>        |       <span data-ttu-id="ee213-194">--таблицы \<имя_таблицы >...</span><span class="sxs-lookup"><span data-stu-id="ee213-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="ee213-195">Таблицы для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="ee213-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="ee213-196">--Используйте имена баз данных</span><span class="sxs-lookup"><span data-stu-id="ee213-196">--use-database-names</span></span>              | <span data-ttu-id="ee213-197">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ee213-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="ee213-198">добавить DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="ee213-198">dotnet ef migrations add</span></span>

<span data-ttu-id="ee213-199">Добавляет новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="ee213-199">Adds a new migration.</span></span>

<span data-ttu-id="ee213-200">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ee213-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="ee213-201">\<ИМЯ ></span><span class="sxs-lookup"><span data-stu-id="ee213-201">\<NAME></span></span> | <span data-ttu-id="ee213-202">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-202">The name of the migration.</span></span> |

<span data-ttu-id="ee213-203">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ee213-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="ee213-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="ee213-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="ee213-205"><nobr>--выходной каталог \<путь ></nobr></span><span class="sxs-lookup"><span data-stu-id="ee213-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="ee213-206">Каталог (и пространства имен sub) для использования.</span><span class="sxs-lookup"><span data-stu-id="ee213-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="ee213-207">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="ee213-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="ee213-208">Значение по умолчанию «Миграция».</span><span class="sxs-lookup"><span data-stu-id="ee213-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="ee213-209">Список миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ee213-209">dotnet ef migrations list</span></span>

<span data-ttu-id="ee213-210">Список доступных миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="ee213-211">Удалите DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="ee213-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="ee213-212">Удаляет последний переноса.</span><span class="sxs-lookup"><span data-stu-id="ee213-212">Removes the last migration.</span></span>

<span data-ttu-id="ee213-213">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ee213-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="ee213-214">-f</span><span class="sxs-lookup"><span data-stu-id="ee213-214">-f</span></span> | <span data-ttu-id="ee213-215">--force</span><span class="sxs-lookup"><span data-stu-id="ee213-215">--force</span></span> | <span data-ttu-id="ee213-216">Не устанавливайте флажок, чтобы проверить, применен ли миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ee213-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="ee213-217">сценарий миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="ee213-217">dotnet ef migrations script</span></span>

<span data-ttu-id="ee213-218">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="ee213-219">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="ee213-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="ee213-220">\<ИЗ ></span><span class="sxs-lookup"><span data-stu-id="ee213-220">\<FROM></span></span> | <span data-ttu-id="ee213-221">Начальный миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-221">The starting migration.</span></span> <span data-ttu-id="ee213-222">По умолчанию равно 0 (начальной базы данных).</span><span class="sxs-lookup"><span data-stu-id="ee213-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="ee213-223">\<ЧТОБЫ ></span><span class="sxs-lookup"><span data-stu-id="ee213-223">\<TO></span></span>   | <span data-ttu-id="ee213-224">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-224">The ending migration.</span></span> <span data-ttu-id="ee213-225">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="ee213-226">Параметры:</span><span class="sxs-lookup"><span data-stu-id="ee213-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="ee213-227">-o</span><span class="sxs-lookup"><span data-stu-id="ee213-227">-o</span></span> | <span data-ttu-id="ee213-228">--Вывод \<файла ></span><span class="sxs-lookup"><span data-stu-id="ee213-228">--output \<FILE></span></span> | <span data-ttu-id="ee213-229">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="ee213-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="ee213-230">-i</span><span class="sxs-lookup"><span data-stu-id="ee213-230">-i</span></span> | <span data-ttu-id="ee213-231">--идемпотентными</span><span class="sxs-lookup"><span data-stu-id="ee213-231">--idempotent</span></span>     | <span data-ttu-id="ee213-232">Создание скрипта, который можно использовать для базы данных на любой миграции.</span><span class="sxs-lookup"><span data-stu-id="ee213-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
