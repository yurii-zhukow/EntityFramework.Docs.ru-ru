---
title: Консоль диспетчера пакетов (Visual Studio) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 0799b0cb7c5d837fdbb7a4af510a9a4d9d34ec1a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949042"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="3d3d3-102">Инструменты консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="3d3d3-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="3d3d3-103">Инструменты консоли диспетчера пакетов (PMC) Core EF работать внутри Visual Studio с помощью NuGet [консоль диспетчера пакетов][2].</span><span class="sxs-lookup"><span data-stu-id="3d3d3-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="3d3d3-104">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="3d3d3-105">Не используете Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="3d3d3-105">Not using Visual Studio?</span></span> <span data-ttu-id="3d3d3-106">[Инструменты командной строки EF Core] [ 1] кросс платформенные и выполнения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="3d3d3-107">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="3d3d3-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="3d3d3-108">Установите инструменты консоли диспетчера пакетов EF Core путем установки пакета Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="3d3d3-109">Его можно установить, выполнив следующую команду в [консоль диспетчера пакетов][2].</span><span class="sxs-lookup"><span data-stu-id="3d3d3-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="3d3d3-110">Если все работает правильно, вы сможете выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="3d3d3-111">Если ваш запускаемый проект предназначен для .NET Standard [переориентироваться на поддерживаемых framework] [ 3] перед использованием средства.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3d3d3-112">Если вы используете **универсальной Windows** или **Xamarin**, переместите свой код EF библиотеку классов .NET Standard и [переориентироваться на поддерживаемых framework] [ 3] перед использованием средства.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="3d3d3-113">Укажите библиотеку класса как запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="3d3d3-114">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="3d3d3-114">Using the tools</span></span>
---------------
<span data-ttu-id="3d3d3-115">При каждом вызове команды состоит из двух проектов:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="3d3d3-116">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="3d3d3-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="3d3d3-117">По умолчанию в целевой проект **проект по умолчанию** выбранный в консоли диспетчера пакетов, но также можно задать с помощью параметра - проекта.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="3d3d3-118">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="3d3d3-119">По умолчанию используется один **Назначить запускаемым проектом** в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="3d3d3-120">Он может также быть указан с помощью параметра - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="3d3d3-121">Общие параметры:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="3d3d3-122">-Контексте \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-122">-Context \<String></span></span>        | <span data-ttu-id="3d3d3-123">DbContext для использования.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="3d3d3-124">— Проект \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-124">-Project \<String></span></span>        | <span data-ttu-id="3d3d3-125">Проект для использования.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-125">The project to use.</span></span>         |
| <span data-ttu-id="3d3d3-126">-StartupProject \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-126">-StartupProject \<String></span></span> | <span data-ttu-id="3d3d3-127">Запускаемый проект для использования.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-127">The startup project to use.</span></span> |
| <span data-ttu-id="3d3d3-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="3d3d3-128">-Verbose</span></span>                  | <span data-ttu-id="3d3d3-129">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-129">Show verbose output.</span></span>        |

<span data-ttu-id="3d3d3-130">Чтобы отобразить справочные сведения о команде, используйте PowerShell `Get-Help` команды.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="3d3d3-131">Параметры контекста, проекта и StartupProject поддерживает расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="3d3d3-132">Задайте **env:ASPNETCORE_ENVIRONMENT** перед запуском для указания среды ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="3d3d3-133">Команды</span><span class="sxs-lookup"><span data-stu-id="3d3d3-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="3d3d3-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="3d3d3-134">Add-Migration</span></span>

<span data-ttu-id="3d3d3-135">Добавление новой миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-135">Adds a new migration.</span></span>

<span data-ttu-id="3d3d3-136">Параметры:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3d3d3-137">***-Name*** \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-137">***-Name*** \<String></span></span>             | <span data-ttu-id="3d3d3-138">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="3d3d3-139"><nobr>-OutputDir \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="3d3d3-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="3d3d3-140">Каталог (и sub-namespace) для использования.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="3d3d3-141">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="3d3d3-142">По умолчанию используется «Миграция».</span><span class="sxs-lookup"><span data-stu-id="3d3d3-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="3d3d3-143">Параметры в **полужирным** являются обязательными и те, которые в *курсив* являются позиционными.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="3d3d3-144">DROP Database</span><span class="sxs-lookup"><span data-stu-id="3d3d3-144">Drop-Database</span></span>

<span data-ttu-id="3d3d3-145">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-145">Drops the database.</span></span>

<span data-ttu-id="3d3d3-146">Параметры:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="3d3d3-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="3d3d3-147">-WhatIf</span></span> | <span data-ttu-id="3d3d3-148">Показать базу данных, которая будет удалена, но не удаляйте ее.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="3d3d3-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="3d3d3-149">Get-DbContext</span></span>

<span data-ttu-id="3d3d3-150">Возвращает сведения о типе DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="3d3d3-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="3d3d3-151">Remove-Migration</span></span>

<span data-ttu-id="3d3d3-152">Удаление последней миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-152">Removes the last migration.</span></span>

<span data-ttu-id="3d3d3-153">Параметры:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="3d3d3-154">-Force</span><span class="sxs-lookup"><span data-stu-id="3d3d3-154">-Force</span></span> | <span data-ttu-id="3d3d3-155">Отменить миграцию, если она была применена к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="3d3d3-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="3d3d3-156">Scaffold-DbContext</span></span>

<span data-ttu-id="3d3d3-157">Формирует шаблоны DbContext и типов сущностей для базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="3d3d3-158">Параметры:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3d3d3-159"><nobr>***-Connection*** \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="3d3d3-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="3d3d3-160">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="3d3d3-161">***-Поставщик*** \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="3d3d3-162">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-162">The provider to use.</span></span> <span data-ttu-id="3d3d3-163">(например, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="3d3d3-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="3d3d3-164">-OutputDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="3d3d3-165">Чтобы поместить файлы каталог.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-165">The directory to put files in.</span></span> <span data-ttu-id="3d3d3-166">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="3d3d3-167">-ContextDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="3d3d3-168">Поместите файл DbContext в каталог.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="3d3d3-169">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="3d3d3-170">-Контексте \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-170">-Context \<String></span></span>                       | <span data-ttu-id="3d3d3-171">Имя для формирования DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="3d3d3-172">-Схемы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="3d3d3-173">Схемы таблиц для создания типов сущности для.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="3d3d3-174">-Таблицы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="3d3d3-175">Для создания типов сущности для таблиц.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="3d3d3-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="3d3d3-176">-DataAnnotations</span></span>                         | <span data-ttu-id="3d3d3-177">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="3d3d3-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="3d3d3-178">Если этот параметр опущен, используется только текучего API.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="3d3d3-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="3d3d3-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="3d3d3-180">Используйте имена таблиц и столбцов непосредственно из базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="3d3d3-181">-Force</span><span class="sxs-lookup"><span data-stu-id="3d3d3-181">-Force</span></span>                                   | <span data-ttu-id="3d3d3-182">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="3d3d3-183">Миграция скриптов</span><span class="sxs-lookup"><span data-stu-id="3d3d3-183">Script-Migration</span></span>

<span data-ttu-id="3d3d3-184">Формирует скрипт SQL из миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="3d3d3-185">Параметры:</span><span class="sxs-lookup"><span data-stu-id="3d3d3-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="3d3d3-186">*-Из* \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-186">*-From* \<String></span></span> | <span data-ttu-id="3d3d3-187">Начала миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-187">The starting migration.</span></span> <span data-ttu-id="3d3d3-188">По умолчанию равно 0 (исходной базы данных).</span><span class="sxs-lookup"><span data-stu-id="3d3d3-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="3d3d3-189">*-* \<Строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-189">*-To* \<String></span></span>   | <span data-ttu-id="3d3d3-190">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-190">The ending migration.</span></span> <span data-ttu-id="3d3d3-191">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="3d3d3-192">-Идемпотентными</span><span class="sxs-lookup"><span data-stu-id="3d3d3-192">-Idempotent</span></span>       | <span data-ttu-id="3d3d3-193">Создайте скрипт, который может использоваться в любой базе данных с любой миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="3d3d3-194">-Выходной \<строка ></span><span class="sxs-lookup"><span data-stu-id="3d3d3-194">-Output \<String></span></span> | <span data-ttu-id="3d3d3-195">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="3d3d3-196">To, From, и выходные параметры поддерживают расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="3d3d3-197">Обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="3d3d3-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="3d3d3-198"><nobr>*-Миграции* \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="3d3d3-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="3d3d3-199">Целевой объект миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-199">The target migration.</span></span> <span data-ttu-id="3d3d3-200">Если значение равно "0", всех операций переноса будут отменены.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="3d3d3-201">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="3d3d3-202">Параметр миграции поддерживает расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="3d3d3-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
