---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="a0296-102">Средства командной строки .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="a0296-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="a0296-103">Средства командной строки .NET Entity Framework Core являются расширением кросс платформенных **dotnet** команду, которая входит в состав из [пакета SDK для .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="a0296-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="a0296-104">Если вы используете Visual Studio, мы рекомендуем [средства PMC] [ 1] , поскольку они предоставляют более интеграция.</span><span class="sxs-lookup"><span data-stu-id="a0296-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a0296-105">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="a0296-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="a0296-106">Установка средств командной строки .NET Core EF, выполнив следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a0296-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="a0296-107">Измените файл проекта и добавьте Microsoft.EntityFrameworkCore.Tools.DotNet как элемент DotNetCliToolReference (см. ниже)</span><span class="sxs-lookup"><span data-stu-id="a0296-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="a0296-108">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="a0296-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="a0296-109">Итоговый проект должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a0296-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="a0296-110">Ссылку на пакет с `PrivateAssets="All"` означает он не предоставлен в проекты, которые ссылаются на этот проект, что особенно полезно для пакетов, которые обычно используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="a0296-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="a0296-111">Если вы все выполнили правильно, необходимо успешно выполните следующую команду в командной строке.</span><span class="sxs-lookup"><span data-stu-id="a0296-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="a0296-112">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="a0296-112">Using the tools</span></span>
---------------
<span data-ttu-id="a0296-113">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="a0296-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a0296-114">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="a0296-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a0296-115">Целевой проект по умолчанию в проект в текущем каталоге, но можно изменить с помощью <nobr> **--проекта** </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="a0296-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="a0296-116">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="a0296-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a0296-117">Он также значения по умолчанию в проект в текущем каталоге, но можно изменить с помощью **--запускаемый проект** параметр.</span><span class="sxs-lookup"><span data-stu-id="a0296-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="a0296-118">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="a0296-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="a0296-119">--json</span><span class="sxs-lookup"><span data-stu-id="a0296-119">--json</span></span>                           | <span data-ttu-id="a0296-120">Отображение выходных данных JSON.</span><span class="sxs-lookup"><span data-stu-id="a0296-120">Show JSON output.</span></span>           |
| <span data-ttu-id="a0296-121">-c</span><span class="sxs-lookup"><span data-stu-id="a0296-121">-c</span></span> | <span data-ttu-id="a0296-122">--контекста \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="a0296-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="a0296-123">Чтобы использовать класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="a0296-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="a0296-124">-p</span><span class="sxs-lookup"><span data-stu-id="a0296-124">-p</span></span> | <span data-ttu-id="a0296-125">--проекта \<проекта ></span><span class="sxs-lookup"><span data-stu-id="a0296-125">--project \<PROJECT></span></span>             | <span data-ttu-id="a0296-126">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="a0296-126">The project to use.</span></span>         |
| <span data-ttu-id="a0296-127">-s</span><span class="sxs-lookup"><span data-stu-id="a0296-127">-s</span></span> | <span data-ttu-id="a0296-128">--запускаемый проект \<проекта ></span><span class="sxs-lookup"><span data-stu-id="a0296-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="a0296-129">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="a0296-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="a0296-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="a0296-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="a0296-131">Требуемая версия .NET framework.</span><span class="sxs-lookup"><span data-stu-id="a0296-131">The target framework.</span></span>       |
|    | <span data-ttu-id="a0296-132">--Конфигурация \<конфигурации ></span><span class="sxs-lookup"><span data-stu-id="a0296-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="a0296-133">Используемая конфигурация.</span><span class="sxs-lookup"><span data-stu-id="a0296-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="a0296-134">Среда выполнения — \<идентификатор ></span><span class="sxs-lookup"><span data-stu-id="a0296-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="a0296-135">Использовать в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="a0296-135">The runtime to use.</span></span>         |
| <span data-ttu-id="a0296-136">-h</span><span class="sxs-lookup"><span data-stu-id="a0296-136">-h</span></span> | <span data-ttu-id="a0296-137">--Справка</span><span class="sxs-lookup"><span data-stu-id="a0296-137">--help</span></span>                           | <span data-ttu-id="a0296-138">Отобразить справку.</span><span class="sxs-lookup"><span data-stu-id="a0296-138">Show help information.</span></span>      |
| <span data-ttu-id="a0296-139">-v</span><span class="sxs-lookup"><span data-stu-id="a0296-139">-v</span></span> | <span data-ttu-id="a0296-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="a0296-140">--verbose</span></span>                        | <span data-ttu-id="a0296-141">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="a0296-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="a0296-142">--Нет цвета</span><span class="sxs-lookup"><span data-stu-id="a0296-142">--no-color</span></span>                       | <span data-ttu-id="a0296-143">Не выделения цветом выходных данных.</span><span class="sxs-lookup"><span data-stu-id="a0296-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="a0296-144">— префикс вывода</span><span class="sxs-lookup"><span data-stu-id="a0296-144">--prefix-output</span></span>                  | <span data-ttu-id="a0296-145">Префикс выходных данных с уровнем.</span><span class="sxs-lookup"><span data-stu-id="a0296-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="a0296-146">Чтобы указать среды ASP.NET Core, задайте **ASPNETCORE_ENVIRONMENT** переменной среды перед запуском.</span><span class="sxs-lookup"><span data-stu-id="a0296-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="a0296-147">Команды</span><span class="sxs-lookup"><span data-stu-id="a0296-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="a0296-148">Удаление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a0296-148">dotnet ef database drop</span></span>

<span data-ttu-id="a0296-149">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="a0296-149">Drops the database.</span></span>

<span data-ttu-id="a0296-150">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a0296-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="a0296-151">-f</span><span class="sxs-lookup"><span data-stu-id="a0296-151">-f</span></span> | <span data-ttu-id="a0296-152">--force</span><span class="sxs-lookup"><span data-stu-id="a0296-152">--force</span></span>   | <span data-ttu-id="a0296-153">Не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="a0296-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="a0296-154">— выполнение</span><span class="sxs-lookup"><span data-stu-id="a0296-154">--dry-run</span></span> | <span data-ttu-id="a0296-155">Показать базу данных, которая будет удалена, но не удалять его.</span><span class="sxs-lookup"><span data-stu-id="a0296-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="a0296-156">Обновление базы данных ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a0296-156">dotnet ef database update</span></span>

<span data-ttu-id="a0296-157">Обновляет базу данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="a0296-158">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a0296-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="a0296-159">\<МИГРАЦИЯ ></span><span class="sxs-lookup"><span data-stu-id="a0296-159">\<MIGRATION></span></span> | <span data-ttu-id="a0296-160">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="a0296-160">The target migration.</span></span> <span data-ttu-id="a0296-161">Если значение равно 0, всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="a0296-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="a0296-162">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="a0296-163">сведения о dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a0296-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="a0296-164">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="a0296-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="a0296-165">Список dbcontext ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a0296-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="a0296-166">Список доступных типов DbContext.</span><span class="sxs-lookup"><span data-stu-id="a0296-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="a0296-167">ef DotNet dbcontext-формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="a0296-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="a0296-168">Закрепление типы DbContext и сущности, для базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0296-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a0296-169">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a0296-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="a0296-170">\<ПОДКЛЮЧЕНИЯ ></span><span class="sxs-lookup"><span data-stu-id="a0296-170">\<CONNECTION></span></span> | <span data-ttu-id="a0296-171">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a0296-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="a0296-172">\<ПОСТАВЩИК ></span><span class="sxs-lookup"><span data-stu-id="a0296-172">\<PROVIDER></span></span>   | <span data-ttu-id="a0296-173">Поставщик для использования.</span><span class="sxs-lookup"><span data-stu-id="a0296-173">The provider to use.</span></span> <span data-ttu-id="a0296-174">(Например)</span><span class="sxs-lookup"><span data-stu-id="a0296-174">(E.g.</span></span> <span data-ttu-id="a0296-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a0296-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="a0296-176">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a0296-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a0296-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="a0296-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="a0296-178">--заметок к данным</span><span class="sxs-lookup"><span data-stu-id="a0296-178">--data-annotations</span></span>                      | <span data-ttu-id="a0296-179">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="a0296-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a0296-180">Если этот параметр опущен, используется только fluent API.</span><span class="sxs-lookup"><span data-stu-id="a0296-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a0296-181">-c</span><span class="sxs-lookup"><span data-stu-id="a0296-181">-c</span></span>              | <span data-ttu-id="a0296-182">--контекста \<имя ></span><span class="sxs-lookup"><span data-stu-id="a0296-182">--context \<NAME></span></span>                       | <span data-ttu-id="a0296-183">Имя DbContext.</span><span class="sxs-lookup"><span data-stu-id="a0296-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="a0296-184">-f</span><span class="sxs-lookup"><span data-stu-id="a0296-184">-f</span></span>              | <span data-ttu-id="a0296-185">--force</span><span class="sxs-lookup"><span data-stu-id="a0296-185">--force</span></span>                                 | <span data-ttu-id="a0296-186">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="a0296-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="a0296-187">-o</span><span class="sxs-lookup"><span data-stu-id="a0296-187">-o</span></span>              | <span data-ttu-id="a0296-188">--выходной каталог \<путь ></span><span class="sxs-lookup"><span data-stu-id="a0296-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="a0296-189">Чтобы поместить файлы в каталог.</span><span class="sxs-lookup"><span data-stu-id="a0296-189">The directory to put files in.</span></span> <span data-ttu-id="a0296-190">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="a0296-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="a0296-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="a0296-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="a0296-192">Схемы таблиц для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="a0296-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a0296-193">-t</span><span class="sxs-lookup"><span data-stu-id="a0296-193">-t</span></span>              | <span data-ttu-id="a0296-194">--таблицы \<имя_таблицы >...</span><span class="sxs-lookup"><span data-stu-id="a0296-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="a0296-195">Таблицы для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="a0296-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="a0296-196">--Используйте имена баз данных</span><span class="sxs-lookup"><span data-stu-id="a0296-196">--use-database-names</span></span>                    | <span data-ttu-id="a0296-197">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0296-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="a0296-198">добавить DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="a0296-198">dotnet ef migrations add</span></span>

<span data-ttu-id="a0296-199">Добавляет новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="a0296-199">Adds a new migration.</span></span>

<span data-ttu-id="a0296-200">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a0296-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="a0296-201">\<ИМЯ ></span><span class="sxs-lookup"><span data-stu-id="a0296-201">\<NAME></span></span> | <span data-ttu-id="a0296-202">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-202">The name of the migration.</span></span> |

<span data-ttu-id="a0296-203">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a0296-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a0296-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="a0296-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="a0296-205"><nobr>--выходной каталог \<путь ></nobr></span><span class="sxs-lookup"><span data-stu-id="a0296-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="a0296-206">Каталог (и пространства имен sub) для использования.</span><span class="sxs-lookup"><span data-stu-id="a0296-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a0296-207">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="a0296-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="a0296-208">Значение по умолчанию «Миграция».</span><span class="sxs-lookup"><span data-stu-id="a0296-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="a0296-209">Список миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a0296-209">dotnet ef migrations list</span></span>

<span data-ttu-id="a0296-210">Список доступных миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="a0296-211">Удалите DotNet ef миграции</span><span class="sxs-lookup"><span data-stu-id="a0296-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="a0296-212">Удаляет последний переноса.</span><span class="sxs-lookup"><span data-stu-id="a0296-212">Removes the last migration.</span></span>

<span data-ttu-id="a0296-213">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a0296-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="a0296-214">-f</span><span class="sxs-lookup"><span data-stu-id="a0296-214">-f</span></span> | <span data-ttu-id="a0296-215">--force</span><span class="sxs-lookup"><span data-stu-id="a0296-215">--force</span></span> | <span data-ttu-id="a0296-216">Не устанавливайте флажок, чтобы проверить, применен ли миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a0296-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="a0296-217">сценарий миграции ef DotNet</span><span class="sxs-lookup"><span data-stu-id="a0296-217">dotnet ef migrations script</span></span>

<span data-ttu-id="a0296-218">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a0296-219">Аргументы:</span><span class="sxs-lookup"><span data-stu-id="a0296-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="a0296-220">\<FROM></span><span class="sxs-lookup"><span data-stu-id="a0296-220">\<FROM></span></span> | <span data-ttu-id="a0296-221">Начальный миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-221">The starting migration.</span></span> <span data-ttu-id="a0296-222">По умолчанию равно 0 (начальной базы данных).</span><span class="sxs-lookup"><span data-stu-id="a0296-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="a0296-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="a0296-223">\<TO></span></span>   | <span data-ttu-id="a0296-224">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-224">The ending migration.</span></span> <span data-ttu-id="a0296-225">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="a0296-226">Параметры:</span><span class="sxs-lookup"><span data-stu-id="a0296-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a0296-227">-o</span><span class="sxs-lookup"><span data-stu-id="a0296-227">-o</span></span> | <span data-ttu-id="a0296-228">--Вывод \<файла ></span><span class="sxs-lookup"><span data-stu-id="a0296-228">--output \<FILE></span></span> | <span data-ttu-id="a0296-229">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="a0296-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="a0296-230">-i</span><span class="sxs-lookup"><span data-stu-id="a0296-230">-i</span></span> | <span data-ttu-id="a0296-231">--идемпотентными</span><span class="sxs-lookup"><span data-stu-id="a0296-231">--idempotent</span></span>     | <span data-ttu-id="a0296-232">Создание скрипта, который можно использовать для базы данных на любой миграции.</span><span class="sxs-lookup"><span data-stu-id="a0296-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
