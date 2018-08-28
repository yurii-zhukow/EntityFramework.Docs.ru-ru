---
title: Консоль диспетчера пакетов (Visual Studio) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.openlocfilehash: 6a3594a3535f8de30ec1898fd21cfcbe272f216b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997938"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="e510e-102">Инструменты консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="e510e-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="e510e-103">Инструменты консоли диспетчера пакетов (PMC) Core EF работать внутри Visual Studio с помощью NuGet [консоль диспетчера пакетов][2].</span><span class="sxs-lookup"><span data-stu-id="e510e-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="e510e-104">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e510e-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="e510e-105">Не используете Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="e510e-105">Not using Visual Studio?</span></span> <span data-ttu-id="e510e-106">[Инструменты командной строки EF Core] [ 1] кросс платформенные и выполнения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="e510e-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="e510e-107">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="e510e-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="e510e-108">Установите инструменты консоли диспетчера пакетов EF Core путем установки пакета Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="e510e-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="e510e-109">Его можно установить, выполнив следующую команду в [консоль диспетчера пакетов][2].</span><span class="sxs-lookup"><span data-stu-id="e510e-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="e510e-110">Если все работает правильно, вы сможете выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e510e-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="e510e-111">Если ваш запускаемый проект предназначен для .NET Standard [переориентироваться на поддерживаемых framework] [ 3] перед использованием средства.</span><span class="sxs-lookup"><span data-stu-id="e510e-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e510e-112">Если вы используете **универсальной Windows** или **Xamarin**, переместите свой код EF библиотеку классов .NET Standard и [переориентироваться на поддерживаемых framework] [ 3] перед использованием средства.</span><span class="sxs-lookup"><span data-stu-id="e510e-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="e510e-113">Укажите библиотеку класса как запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="e510e-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="e510e-114">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="e510e-114">Using the tools</span></span>
---------------
<span data-ttu-id="e510e-115">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="e510e-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="e510e-116">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="e510e-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="e510e-117">По умолчанию в целевой проект **проект по умолчанию** выбранный в консоли диспетчера пакетов, но также можно задать с помощью параметра - проекта.</span><span class="sxs-lookup"><span data-stu-id="e510e-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="e510e-118">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="e510e-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="e510e-119">По умолчанию используется один **Назначить запускаемым проектом** в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="e510e-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="e510e-120">Он может также быть указан с помощью параметра - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="e510e-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="e510e-121">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="e510e-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="e510e-122">-Контексте \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-122">-Context \<String></span></span>        | <span data-ttu-id="e510e-123">DbContext для использования.</span><span class="sxs-lookup"><span data-stu-id="e510e-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="e510e-124">— Проект \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-124">-Project \<String></span></span>        | <span data-ttu-id="e510e-125">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="e510e-125">The project to use.</span></span>         |
| <span data-ttu-id="e510e-126">-StartupProject \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-126">-StartupProject \<String></span></span> | <span data-ttu-id="e510e-127">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="e510e-127">The startup project to use.</span></span> |
| <span data-ttu-id="e510e-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="e510e-128">-Verbose</span></span>                  | <span data-ttu-id="e510e-129">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="e510e-129">Show verbose output.</span></span>        |

<span data-ttu-id="e510e-130">Чтобы отобразить справочные сведения о команде, используйте PowerShell `Get-Help` команды.</span><span class="sxs-lookup"><span data-stu-id="e510e-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="e510e-131">Параметры контекста, проекта и StartupProject поддерживает расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="e510e-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="e510e-132">Задайте **env:ASPNETCORE_ENVIRONMENT** перед запуском для указания среды ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e510e-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="e510e-133">Команды</span><span class="sxs-lookup"><span data-stu-id="e510e-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="e510e-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="e510e-134">Add-Migration</span></span>

<span data-ttu-id="e510e-135">Добавление новой миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-135">Adds a new migration.</span></span>

<span data-ttu-id="e510e-136">Параметры:</span><span class="sxs-lookup"><span data-stu-id="e510e-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e510e-137">***-Name*** \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-137">***-Name*** \<String></span></span>             | <span data-ttu-id="e510e-138">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="e510e-139"><nobr>-OutputDir \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="e510e-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="e510e-140">Каталог (и sub-namespace) для использования.</span><span class="sxs-lookup"><span data-stu-id="e510e-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="e510e-141">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="e510e-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="e510e-142">По умолчанию используется «Миграция».</span><span class="sxs-lookup"><span data-stu-id="e510e-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="e510e-143">Параметры в **полужирным** являются обязательными и те, которые в *курсив* являются позиционными.</span><span class="sxs-lookup"><span data-stu-id="e510e-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="e510e-144">DROP Database</span><span class="sxs-lookup"><span data-stu-id="e510e-144">Drop-Database</span></span>

<span data-ttu-id="e510e-145">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="e510e-145">Drops the database.</span></span>

<span data-ttu-id="e510e-146">Параметры:</span><span class="sxs-lookup"><span data-stu-id="e510e-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="e510e-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="e510e-147">-WhatIf</span></span> | <span data-ttu-id="e510e-148">Показать базу данных, которая будет удалена, но не удаляйте ее.</span><span class="sxs-lookup"><span data-stu-id="e510e-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="e510e-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="e510e-149">Get-DbContext</span></span>

<span data-ttu-id="e510e-150">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="e510e-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="e510e-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="e510e-151">Remove-Migration</span></span>

<span data-ttu-id="e510e-152">Удаление последней миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-152">Removes the last migration.</span></span>

<span data-ttu-id="e510e-153">Параметры:</span><span class="sxs-lookup"><span data-stu-id="e510e-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="e510e-154">-Force</span><span class="sxs-lookup"><span data-stu-id="e510e-154">-Force</span></span> | <span data-ttu-id="e510e-155">Отменить миграцию, если она была применена к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e510e-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="e510e-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="e510e-156">Scaffold-DbContext</span></span>

<span data-ttu-id="e510e-157">Формирует шаблоны DbContext и типов сущностей для базы данных.</span><span class="sxs-lookup"><span data-stu-id="e510e-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="e510e-158">Параметры:</span><span class="sxs-lookup"><span data-stu-id="e510e-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e510e-159"><nobr>***-Connection*** \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="e510e-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="e510e-160">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e510e-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="e510e-161">***-Поставщик*** \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="e510e-162">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="e510e-162">The provider to use.</span></span> <span data-ttu-id="e510e-163">(например, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="e510e-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="e510e-164">-OutputDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="e510e-165">Чтобы поместить файлы каталог.</span><span class="sxs-lookup"><span data-stu-id="e510e-165">The directory to put files in.</span></span> <span data-ttu-id="e510e-166">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="e510e-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="e510e-167">-ContextDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="e510e-168">Поместите файл DbContext в каталог.</span><span class="sxs-lookup"><span data-stu-id="e510e-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="e510e-169">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="e510e-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="e510e-170">-Контексте \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-170">-Context \<String></span></span>                       | <span data-ttu-id="e510e-171">Имя для формирования DbContext.</span><span class="sxs-lookup"><span data-stu-id="e510e-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="e510e-172">-Схемы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="e510e-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="e510e-173">Схемы таблиц для создания типов сущности для.</span><span class="sxs-lookup"><span data-stu-id="e510e-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="e510e-174">-Таблицы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="e510e-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="e510e-175">Для создания типов сущности для таблиц.</span><span class="sxs-lookup"><span data-stu-id="e510e-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="e510e-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="e510e-176">-DataAnnotations</span></span>                         | <span data-ttu-id="e510e-177">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="e510e-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="e510e-178">Если этот параметр опущен, используется только текучего API.</span><span class="sxs-lookup"><span data-stu-id="e510e-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="e510e-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="e510e-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="e510e-180">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e510e-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="e510e-181">-Force</span><span class="sxs-lookup"><span data-stu-id="e510e-181">-Force</span></span>                                   | <span data-ttu-id="e510e-182">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="e510e-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="e510e-183">Миграция скриптов</span><span class="sxs-lookup"><span data-stu-id="e510e-183">Script-Migration</span></span>

<span data-ttu-id="e510e-184">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="e510e-185">Параметры:</span><span class="sxs-lookup"><span data-stu-id="e510e-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="e510e-186">*-Из* \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-186">*-From* \<String></span></span> | <span data-ttu-id="e510e-187">Начала миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-187">The starting migration.</span></span> <span data-ttu-id="e510e-188">По умолчанию равно 0 (исходной базы данных).</span><span class="sxs-lookup"><span data-stu-id="e510e-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="e510e-189">*-* \<Строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-189">*-To* \<String></span></span>   | <span data-ttu-id="e510e-190">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-190">The ending migration.</span></span> <span data-ttu-id="e510e-191">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="e510e-192">-Идемпотентными</span><span class="sxs-lookup"><span data-stu-id="e510e-192">-Idempotent</span></span>       | <span data-ttu-id="e510e-193">Создайте скрипт, который может использоваться в любой базе данных с любой миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="e510e-194">-Выходной \<строка ></span><span class="sxs-lookup"><span data-stu-id="e510e-194">-Output \<String></span></span> | <span data-ttu-id="e510e-195">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="e510e-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="e510e-196">To, From, и выходные параметры поддерживают расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="e510e-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="e510e-197">Обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="e510e-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="e510e-198"><nobr>*-Миграции* \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="e510e-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="e510e-199">Целевой объект миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-199">The target migration.</span></span> <span data-ttu-id="e510e-200">Если значение равно "0", всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="e510e-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="e510e-201">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="e510e-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="e510e-202">Параметр миграции поддерживает расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="e510e-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
