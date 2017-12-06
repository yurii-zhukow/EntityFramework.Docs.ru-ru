---
title: "Консоль диспетчера пакетов (Visual Studio) - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="f2ab8-102">Средства консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="f2ab8-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="f2ab8-103">Средства EF основных компонентов пакета диспетчера консоли (PMC) выполняются внутри Visual Studio с помощью NuGet [консоль диспетчера пакетов][2].</span><span class="sxs-lookup"><span data-stu-id="f2ab8-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="f2ab8-104">Эти средства работы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="f2ab8-105">Не используете Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="f2ab8-105">Not using Visual Studio?</span></span> <span data-ttu-id="f2ab8-106">[EF основные инструменты командной строки] [ 1] являются между различными платформами и работают в командную строку.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="f2ab8-107">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="f2ab8-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="f2ab8-108">Установите средства консоли диспетчера пакетов Core EF путем установки пакета Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="f2ab8-109">Можно установить его, выполнив следующую команду в [консоль диспетчера пакетов][2].</span><span class="sxs-lookup"><span data-stu-id="f2ab8-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="f2ab8-110">Если все работает правильно, можно выполнить эту команду:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="f2ab8-111">Если автоматически запускаемый проект предназначен для .NET Standard [кросс целевой поддерживаемых платформ] [ 3] перед использованием средства.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2ab8-112">Если вы используете **универсальных приложений Windows** или **Xamarin**, переместите свой код EF в библиотеке классов .NET Standard и [кросс целевой поддерживаемых платформ] [ 3] перед использованием средства.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="f2ab8-113">Укажите в качестве запускаемого проекта библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="f2ab8-114">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="f2ab8-114">Using the tools</span></span>
---------------
<span data-ttu-id="f2ab8-115">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="f2ab8-116">Целевой проект — там, где добавляются все файлы (или в некоторых случаях удалить).</span><span class="sxs-lookup"><span data-stu-id="f2ab8-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="f2ab8-117">По умолчанию в целевой проект **проекта по умолчанию** выбранный в консоли диспетчера пакетов, но также можно задать с помощью параметра - проекта.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="f2ab8-118">Запускаемый проект является эмулировать средствами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="f2ab8-119">По умолчанию используется один **Назначить запускаемым проектом** в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="f2ab8-120">Он может также быть указан с помощью параметра - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="f2ab8-121">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-121">Common parameters:</span></span>

|                           |                             |
| ------------------------- | --------------------------- |
| <span data-ttu-id="f2ab8-122">-Контекст \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-122">-Context \<String></span></span>        | <span data-ttu-id="f2ab8-123">Чтобы использовать класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="f2ab8-124">-Проект \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-124">-Project \<String></span></span>        | <span data-ttu-id="f2ab8-125">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-125">The project to use.</span></span>         |
| <span data-ttu-id="f2ab8-126">-StartupProject \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-126">-StartupProject \<String></span></span> | <span data-ttu-id="f2ab8-127">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-127">The startup project to use.</span></span> |
| <span data-ttu-id="f2ab8-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="f2ab8-128">-Verbose</span></span>                  | <span data-ttu-id="f2ab8-129">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-129">Show verbose output.</span></span>        |

<span data-ttu-id="f2ab8-130">Чтобы отобразить справку по команде, используйте PowerShell `Get-Help` команды.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="f2ab8-131">Параметры контекста, проекта и StartupProject поддерживает расширение по клавише tab.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="f2ab8-132">Задать **env:ASPNETCORE_ENVIRONMENT** перед запуском для указания среды ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="f2ab8-133">Команды</span><span class="sxs-lookup"><span data-stu-id="f2ab8-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="f2ab8-134">Добавить миграции</span><span class="sxs-lookup"><span data-stu-id="f2ab8-134">Add-Migration</span></span>

<span data-ttu-id="f2ab8-135">Добавляет новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-135">Adds a new migration.</span></span>

<span data-ttu-id="f2ab8-136">Параметры:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-136">Parameters:</span></span>

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| <span data-ttu-id="f2ab8-137">***-Имя*** \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-137">***-Name*** \<String></span></span>              | <span data-ttu-id="f2ab8-138">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-138">The name of the migration.</span></span>                                                      |
| <span data-ttu-id="f2ab8-139"><nobr>-OutputDir \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="f2ab8-139"><nobr>-OutputDir \<String></nobr></span></span>  | <span data-ttu-id="f2ab8-140">Каталог (и пространства имен sub) для использования.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="f2ab8-141">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="f2ab8-142">Значение по умолчанию «Миграция».</span><span class="sxs-lookup"><span data-stu-id="f2ab8-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="f2ab8-143">Параметры в **полужирным** являются обязательными и версии в *курсив* являются позиционным.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="f2ab8-144">Удаление-базы данных</span><span class="sxs-lookup"><span data-stu-id="f2ab8-144">Drop-Database</span></span>

<span data-ttu-id="f2ab8-145">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-145">Drops the database.</span></span>

<span data-ttu-id="f2ab8-146">Параметры:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-146">Parameters:</span></span>

|          |                                                          |
| -------- | -------------------------------------------------------- |
| <span data-ttu-id="f2ab8-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="f2ab8-147">-WhatIf</span></span>  | <span data-ttu-id="f2ab8-148">Показать базу данных, которая будет удалена, но не удалять его.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="f2ab8-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="f2ab8-149">Get-DbContext</span></span>

<span data-ttu-id="f2ab8-150">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="f2ab8-151">Удаление миграции</span><span class="sxs-lookup"><span data-stu-id="f2ab8-151">Remove-Migration</span></span>

<span data-ttu-id="f2ab8-152">Удаляет последний переноса.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-152">Removes the last migration.</span></span>

<span data-ttu-id="f2ab8-153">Параметры:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-153">Parameters:</span></span>

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| <span data-ttu-id="f2ab8-154">-Force</span><span class="sxs-lookup"><span data-stu-id="f2ab8-154">-Force</span></span> | <span data-ttu-id="f2ab8-155">Не устанавливайте флажок, чтобы проверить, применен ли миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="f2ab8-156">DbContext формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="f2ab8-156">Scaffold-DbContext</span></span>

<span data-ttu-id="f2ab8-157">Закрепление типы DbContext и сущности, для базы данных.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="f2ab8-158">Параметры:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-158">Parameters:</span></span>

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <span data-ttu-id="f2ab8-159"><nobr>***-Подключения*** \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="f2ab8-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="f2ab8-160">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-160">The connection string to the database.</span></span>                                    |
| <span data-ttu-id="f2ab8-161">***-Поставщик*** \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="f2ab8-162">Поставщик для использования.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-162">The provider to use.</span></span> <span data-ttu-id="f2ab8-163">(Пример</span><span class="sxs-lookup"><span data-stu-id="f2ab8-163">(E.g.</span></span> <span data-ttu-id="f2ab8-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="f2ab8-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>       |
| <span data-ttu-id="f2ab8-165">-OutputDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="f2ab8-166">Чтобы поместить файлы в каталог.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-166">The directory to put files in.</span></span> <span data-ttu-id="f2ab8-167">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-167">Paths are relative to the project directory.</span></span> |
| <span data-ttu-id="f2ab8-168">-Контекст \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-168">-Context \<String></span></span>                       | <span data-ttu-id="f2ab8-169">Имя DbContext для создания.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-169">The name of the DbContext to generate.</span></span>                                    |
| <span data-ttu-id="f2ab8-170">-Схемы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="f2ab8-171">Схемы таблиц для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-171">The schemas of tables to generate entity types for.</span></span>                       |
| <span data-ttu-id="f2ab8-172">-Таблицы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="f2ab8-173">Таблицы для создания типов сущностей для.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-173">The tables to generate entity types for.</span></span>                                  |
| <span data-ttu-id="f2ab8-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="f2ab8-174">-DataAnnotations</span></span>                         | <span data-ttu-id="f2ab8-175">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="f2ab8-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="f2ab8-176">Если этот параметр опущен, используется только fluent API.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="f2ab8-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="f2ab8-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="f2ab8-178">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-178">Use table and column names directly from the database.</span></span>                    |
| <span data-ttu-id="f2ab8-179">-Force</span><span class="sxs-lookup"><span data-stu-id="f2ab8-179">-Force</span></span>                                   | <span data-ttu-id="f2ab8-180">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-180">Overwrite existing files.</span></span>                                                 |

### <a name="script-migration"></a><span data-ttu-id="f2ab8-181">Миграция скриптов</span><span class="sxs-lookup"><span data-stu-id="f2ab8-181">Script-Migration</span></span>

<span data-ttu-id="f2ab8-182">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="f2ab8-183">Параметры:</span><span class="sxs-lookup"><span data-stu-id="f2ab8-183">Parameters:</span></span>

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="f2ab8-184">*-Из* \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-184">*-From* \<String></span></span> | <span data-ttu-id="f2ab8-185">Начальный миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-185">The starting migration.</span></span> <span data-ttu-id="f2ab8-186">По умолчанию равно 0 (начальной базы данных).</span><span class="sxs-lookup"><span data-stu-id="f2ab8-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="f2ab8-187">*-* \<Строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-187">*-To* \<String></span></span>   | <span data-ttu-id="f2ab8-188">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-188">The ending migration.</span></span> <span data-ttu-id="f2ab8-189">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="f2ab8-190">-Идемпотентными</span><span class="sxs-lookup"><span data-stu-id="f2ab8-190">-Idempotent</span></span>       | <span data-ttu-id="f2ab8-191">Создание скрипта, который можно использовать для базы данных на любой миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="f2ab8-192">-Output \<строка ></span><span class="sxs-lookup"><span data-stu-id="f2ab8-192">-Output \<String></span></span> | <span data-ttu-id="f2ab8-193">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="f2ab8-194">To, From, и выходные параметры поддержки расширение по клавише tab.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="f2ab8-195">Update-Database</span><span class="sxs-lookup"><span data-stu-id="f2ab8-195">Update-Database</span></span>

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="f2ab8-196"><nobr>*-Миграция* \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="f2ab8-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="f2ab8-197">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-197">The target migration.</span></span> <span data-ttu-id="f2ab8-198">Если значение равно "0", всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="f2ab8-199">По умолчанию последней миграции.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="f2ab8-200">Параметр миграции поддерживает расширение по клавише tab.</span><span class="sxs-lookup"><span data-stu-id="f2ab8-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
