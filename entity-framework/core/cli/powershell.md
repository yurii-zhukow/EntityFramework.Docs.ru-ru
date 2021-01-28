---
title: Справочник по средствам EF Core (консоль диспетчера пакетов) — EF Core
description: Справочное руководство по консоли диспетчера пакетов Visual Studio Entity Framework Core
author: bricelam
ms.date: 10/27/2020
uid: core/cli/powershell
ms.openlocfilehash: 51f2dbcad0606a0d5571e96d7a7951cf595e2b9e
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983512"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="26a3e-103">Справочник по средствам Entity Framework Core. консоль диспетчера пакетов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26a3e-103">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="26a3e-104">Средства консоли диспетчера пакетов (PMC) для Entity Framework Core выполнения задач разработки во время разработки.</span><span class="sxs-lookup"><span data-stu-id="26a3e-104">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="26a3e-105">Например, они создают [миграции](/aspnet/core/data/ef-mvc/migrations), применяют миграции и создают код для модели на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-105">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="26a3e-106">Команды выполняются в Visual Studio с помощью [консоли диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="26a3e-106">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="26a3e-107">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="26a3e-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="26a3e-108">Если вы не используете Visual Studio, вместо этого рекомендуется использовать [средства командной строки EF Core](xref:core/cli/dotnet) .</span><span class="sxs-lookup"><span data-stu-id="26a3e-108">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](xref:core/cli/dotnet) instead.</span></span> <span data-ttu-id="26a3e-109">Средства .NET Core CLI являются кросс-платформенными и выполняются в командной строке.</span><span class="sxs-lookup"><span data-stu-id="26a3e-109">The .NET Core CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="26a3e-110">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="26a3e-110">Installing the tools</span></span>

<span data-ttu-id="26a3e-111">Установите средства консоли диспетчера пакетов, выполнив следующую команду в **консоли диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="26a3e-111">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="26a3e-112">Обновите средства, выполнив следующую команду в **консоли диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="26a3e-112">Update the tools by running the following command in **Package Manager Console**.</span></span>

```powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="26a3e-113">Проверка установки</span><span class="sxs-lookup"><span data-stu-id="26a3e-113">Verify the installation</span></span>

<span data-ttu-id="26a3e-114">Убедитесь, что средства установлены, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="26a3e-114">Verify that the tools are installed by running this command:</span></span>

```powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="26a3e-115">Выходные данные выглядят следующим образом (он не указывает, какую версию инструментов вы используете):</span><span class="sxs-lookup"><span data-stu-id="26a3e-115">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```output

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="26a3e-116">Использование средств</span><span class="sxs-lookup"><span data-stu-id="26a3e-116">Using the tools</span></span>

<span data-ttu-id="26a3e-117">Перед использованием средств:</span><span class="sxs-lookup"><span data-stu-id="26a3e-117">Before using the tools:</span></span>

* <span data-ttu-id="26a3e-118">Изучите разницу между целевым и запускаемым проектами.</span><span class="sxs-lookup"><span data-stu-id="26a3e-118">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="26a3e-119">Узнайте, как использовать средства с библиотеками классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="26a3e-119">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="26a3e-120">Для проектов ASP.NET Core задайте среду.</span><span class="sxs-lookup"><span data-stu-id="26a3e-120">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="26a3e-121">Целевой и запускаемый проекты</span><span class="sxs-lookup"><span data-stu-id="26a3e-121">Target and startup project</span></span>

<span data-ttu-id="26a3e-122">Команды ссылаются на *проект* и *запускаемый проект*.</span><span class="sxs-lookup"><span data-stu-id="26a3e-122">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="26a3e-123">*Проект* также называется *целевым проектом* , так как он служит для добавления или удаления файлов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-123">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="26a3e-124">По умолчанию **проект** , выбранный в **консоли диспетчера пакетов** , является целевым.</span><span class="sxs-lookup"><span data-stu-id="26a3e-124">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="26a3e-125">Можно указать другой проект в качестве целевого проекта с помощью <nobr>`--project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="26a3e-125">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="26a3e-126">*Запускаемый проект* — это тот, который выполняет сборка и запуск средств.</span><span class="sxs-lookup"><span data-stu-id="26a3e-126">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="26a3e-127">Средства должны выполнять код приложения во время разработки для получения сведений о проекте, таких как строка подключения к базе данных и Конфигурация модели.</span><span class="sxs-lookup"><span data-stu-id="26a3e-127">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="26a3e-128">По умолчанию **запускаемым проектом** в **Обозреватель решений** является запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="26a3e-128">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="26a3e-129">Можно указать другой проект в качестве запускаемого проекта с помощью <nobr>`--startup-project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="26a3e-129">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="26a3e-130">Запускаемый проект и целевой проект часто являются одним и тем же проектом.</span><span class="sxs-lookup"><span data-stu-id="26a3e-130">The startup project and target project are often the same project.</span></span> <span data-ttu-id="26a3e-131">Типичный сценарий, в котором они являются отдельными проектами, состоит в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="26a3e-131">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="26a3e-132">Контекст EF Core и классы сущностей находятся в библиотеке классов .NET Core.</span><span class="sxs-lookup"><span data-stu-id="26a3e-132">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="26a3e-133">Консольное приложение .NET Core или веб-приложение ссылается на библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-133">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="26a3e-134">Также можно [разместить код миграции в библиотеке классов, отдельном от контекста EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="26a3e-134">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="26a3e-135">Другие целевые платформы</span><span class="sxs-lookup"><span data-stu-id="26a3e-135">Other target frameworks</span></span>

<span data-ttu-id="26a3e-136">Средства консоли диспетчера пакетов работают с проектами .NET Core или платформа .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="26a3e-136">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="26a3e-137">Приложения с моделью EF Core в библиотеке классов .NET Standard могут не иметь проекта .NET Core или платформа .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="26a3e-137">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="26a3e-138">Например, это справедливо для приложений Xamarin и универсальная платформа Windows.</span><span class="sxs-lookup"><span data-stu-id="26a3e-138">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="26a3e-139">В таких случаях можно создать проект консольного приложения .NET Core или платформа .NET Framework, предназначенное только для запуска в качестве запускаемого проекта для инструментов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-139">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="26a3e-140">Проект может быть фиктивным проектом без реального кода, &mdash; поэтому он необходим только для предоставления целей для инструментов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-140">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="26a3e-141">Зачем нужен фиктивный проект?</span><span class="sxs-lookup"><span data-stu-id="26a3e-141">Why is a dummy project required?</span></span> <span data-ttu-id="26a3e-142">Как упоминалось ранее, средства должны выполнять код приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="26a3e-142">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="26a3e-143">Для этого необходимо использовать среду выполнения .NET Core или платформа .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="26a3e-143">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="26a3e-144">Если EF Coreная модель находится в проекте, ориентированном на .NET Core или платформа .NET Framework, EF Core средства позаимствованы среду выполнения из проекта.</span><span class="sxs-lookup"><span data-stu-id="26a3e-144">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="26a3e-145">Они не могут сделать это, если модель EF Core находится в библиотеке классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="26a3e-145">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="26a3e-146">.NET Standard не является реальной реализацией .NET; это спецификация набора интерфейсов API, которые должны поддерживаться реализациями .NET.</span><span class="sxs-lookup"><span data-stu-id="26a3e-146">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="26a3e-147">Поэтому .NET Standard недостаточно для того, чтобы средства EF Core выполняли код приложения.</span><span class="sxs-lookup"><span data-stu-id="26a3e-147">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="26a3e-148">Фиктивный проект, который создается для использования в качестве запускаемого проекта, предоставляет конкретную целевую платформу, в которую средства могут загрузить библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="26a3e-148">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="26a3e-149">Среда ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26a3e-149">ASP.NET Core environment</span></span>

<span data-ttu-id="26a3e-150">Чтобы указать [среду](/aspnet/core/fundamentals/environments) для проектов ASP.NET Core, установите параметр **env: ASPNETCORE_ENVIRONMENT** перед выполнением команд.</span><span class="sxs-lookup"><span data-stu-id="26a3e-150">To specify [the environment](/aspnet/core/fundamentals/environments) for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

<span data-ttu-id="26a3e-151">Начиная с EF Core 5,0, дополнительные аргументы также могут передаваться в Program. Креатехостбуилдер, что позволяет указать среду в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26a3e-151">Starting in EF Core 5.0, additional arguments can also be passed into Program.CreateHostBuilder allowing you to specify the environment on the command-line:</span></span>

```powershell
Update-Database -Args '--environment Production'
```

## <a name="common-parameters"></a><span data-ttu-id="26a3e-152">Общие параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-152">Common parameters</span></span>

<span data-ttu-id="26a3e-153">В следующей таблице приведены параметры, которые являются общими для всех EF Coreных команд.</span><span class="sxs-lookup"><span data-stu-id="26a3e-153">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="26a3e-154">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-154">Parameter</span></span>                 | <span data-ttu-id="26a3e-155">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-155">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26a3e-156">Контекст \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-156">-Context \<String></span></span>        | <span data-ttu-id="26a3e-157">Класс `DbContext` для использования.</span><span class="sxs-lookup"><span data-stu-id="26a3e-157">The `DbContext` class to use.</span></span> <span data-ttu-id="26a3e-158">Только имя класса или полный квалификатор с пространствами имен.</span><span class="sxs-lookup"><span data-stu-id="26a3e-158">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="26a3e-159">Если этот параметр опущен, EF Core находит класс контекста.</span><span class="sxs-lookup"><span data-stu-id="26a3e-159">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="26a3e-160">Если имеется несколько классов контекста, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="26a3e-160">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="26a3e-161">-Проект \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-161">-Project \<String></span></span>        | <span data-ttu-id="26a3e-162">Целевой проект.</span><span class="sxs-lookup"><span data-stu-id="26a3e-162">The target project.</span></span> <span data-ttu-id="26a3e-163">Если этот параметр не указан, в качестве целевого проекта используется **проект по умолчанию** для **консоли диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="26a3e-163">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="26a3e-164"><nobr>-Стартуппрожект</nobr>\<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-164"><nobr>-StartupProject</nobr> \<String></span></span> | <span data-ttu-id="26a3e-165">Автоматически запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="26a3e-165">The startup project.</span></span> <span data-ttu-id="26a3e-166">Если этот параметр не указан, в качестве целевого проекта используется **запускаемый проект** в **свойствах решения** .</span><span class="sxs-lookup"><span data-stu-id="26a3e-166">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="26a3e-167">-Args \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-167">-Args \<String></span></span>           | <span data-ttu-id="26a3e-168">Аргументы, передаваемые в приложение.</span><span class="sxs-lookup"><span data-stu-id="26a3e-168">Arguments passed to the application.</span></span> <span data-ttu-id="26a3e-169">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-169">Added in EF Core 5.0.</span></span>                                                                                                                                                           |
| <span data-ttu-id="26a3e-170">-Verbose</span><span class="sxs-lookup"><span data-stu-id="26a3e-170">-Verbose</span></span>                  | <span data-ttu-id="26a3e-171">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-171">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="26a3e-172">Чтобы отобразить справочные сведения о команде, используйте команду PowerShell `Get-Help` .</span><span class="sxs-lookup"><span data-stu-id="26a3e-172">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="26a3e-173">Параметры Context, Project и Стартуппрожект поддерживают расширение Tab.</span><span class="sxs-lookup"><span data-stu-id="26a3e-173">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="26a3e-174">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="26a3e-174">Add-Migration</span></span>

<span data-ttu-id="26a3e-175">Добавляет новый перенос.</span><span class="sxs-lookup"><span data-stu-id="26a3e-175">Adds a new migration.</span></span>

<span data-ttu-id="26a3e-176">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-176">Parameters:</span></span>

| <span data-ttu-id="26a3e-177">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-177">Parameter</span></span>                         | <span data-ttu-id="26a3e-178">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-178">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26a3e-179"><nobr>-Имя \<String><nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-179"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="26a3e-180">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-180">The name of the migration.</span></span> <span data-ttu-id="26a3e-181">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="26a3e-181">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="26a3e-182"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-182"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="26a3e-183">Каталог используется для вывода файлов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-183">The directory use to output the files.</span></span> <span data-ttu-id="26a3e-184">Пути задаются относительно целевого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="26a3e-184">Paths are relative to the target project directory.</span></span> <span data-ttu-id="26a3e-185">По умолчанию используется значение "миграции".</span><span class="sxs-lookup"><span data-stu-id="26a3e-185">Defaults to "Migrations".</span></span> |
| <span data-ttu-id="26a3e-186"><nobr>-Namespace \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-186"><nobr>-Namespace \<String></nobr></span></span> | <span data-ttu-id="26a3e-187">Пространство имен, используемое для создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-187">The namespace to use for the generated classes.</span></span> <span data-ttu-id="26a3e-188">По умолчанию создается из выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="26a3e-188">Defaults to generated from the output directory.</span></span> <span data-ttu-id="26a3e-189">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-189">Added in EF Core 5.0.</span></span>  |

<span data-ttu-id="26a3e-190">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-190">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="drop-database"></a><span data-ttu-id="26a3e-191">Drop-Database</span><span class="sxs-lookup"><span data-stu-id="26a3e-191">Drop-Database</span></span>

<span data-ttu-id="26a3e-192">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-192">Drops the database.</span></span>

<span data-ttu-id="26a3e-193">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-193">Parameters:</span></span>

| <span data-ttu-id="26a3e-194">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-194">Parameter</span></span> | <span data-ttu-id="26a3e-195">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-195">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="26a3e-196">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="26a3e-196">-WhatIf</span></span>   | <span data-ttu-id="26a3e-197">Показывает, какую базу данных следует удалить, но не удалять.</span><span class="sxs-lookup"><span data-stu-id="26a3e-197">Show which database would be dropped, but don't drop it.</span></span> |

<span data-ttu-id="26a3e-198">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-198">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="get-dbcontext"></a><span data-ttu-id="26a3e-199">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="26a3e-199">Get-DbContext</span></span>

<span data-ttu-id="26a3e-200">Перечисляет и получает сведения о доступных `DbContext` типах.</span><span class="sxs-lookup"><span data-stu-id="26a3e-200">Lists and gets information about available `DbContext` types.</span></span>

<span data-ttu-id="26a3e-201">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-201">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="get-migration"></a><span data-ttu-id="26a3e-202">Get-Migration</span><span class="sxs-lookup"><span data-stu-id="26a3e-202">Get-Migration</span></span>

<span data-ttu-id="26a3e-203">Список доступных миграций.</span><span class="sxs-lookup"><span data-stu-id="26a3e-203">Lists available migrations.</span></span> <span data-ttu-id="26a3e-204">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-204">Added in EF Core 5.0.</span></span>

<span data-ttu-id="26a3e-205">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-205">Parameters:</span></span>

| <span data-ttu-id="26a3e-206">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-206">Parameter</span></span>                          | <span data-ttu-id="26a3e-207">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-207">Description</span></span>                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------ |
| <span data-ttu-id="26a3e-208"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-208"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="26a3e-209">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-209">The connection string to the database.</span></span> <span data-ttu-id="26a3e-210">По умолчанию используется значение, указанное в AddDbContext или onconfiguring.</span><span class="sxs-lookup"><span data-stu-id="26a3e-210">Defaults to the one specified in AddDbContext or OnConfiguring.</span></span> |
| <span data-ttu-id="26a3e-211">-Соединение</span><span class="sxs-lookup"><span data-stu-id="26a3e-211">-NoConnect</span></span>                         | <span data-ttu-id="26a3e-212">Не подключайтесь к базе данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-212">Don't connect to the database.</span></span>                                                                         |

<span data-ttu-id="26a3e-213">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-213">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="26a3e-214">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="26a3e-214">Remove-Migration</span></span>

<span data-ttu-id="26a3e-215">Удаляет последнюю миграцию (выполняет откат изменений кода, выполненных для миграции).</span><span class="sxs-lookup"><span data-stu-id="26a3e-215">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="26a3e-216">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-216">Parameters:</span></span>

| <span data-ttu-id="26a3e-217">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-217">Parameter</span></span> | <span data-ttu-id="26a3e-218">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-218">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="26a3e-219">-Force</span><span class="sxs-lookup"><span data-stu-id="26a3e-219">-Force</span></span>    | <span data-ttu-id="26a3e-220">Отмените миграцию (выполните откат изменений, примененных к базе данных).</span><span class="sxs-lookup"><span data-stu-id="26a3e-220">Revert the migration (roll back the changes that were applied to the database).</span></span> |

<span data-ttu-id="26a3e-221">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-221">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="scaffold-dbcontext"></a><span data-ttu-id="26a3e-222">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="26a3e-222">Scaffold-DbContext</span></span>

<span data-ttu-id="26a3e-223">Создает код для `DbContext` типов сущностей и для базы данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-223">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="26a3e-224">`Scaffold-DbContext`Чтобы создать тип сущности, таблица базы данных должна иметь первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="26a3e-224">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="26a3e-225">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-225">Parameters:</span></span>

| <span data-ttu-id="26a3e-226">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-226">Parameter</span></span>                          | <span data-ttu-id="26a3e-227">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-227">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26a3e-228"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-228"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="26a3e-229">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-229">The connection string to the database.</span></span> <span data-ttu-id="26a3e-230">Для проектов ASP.NET Core 2. x значением может быть *Name = \<name of connection string>*.</span><span class="sxs-lookup"><span data-stu-id="26a3e-230">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="26a3e-231">В этом случае имя берется из источников конфигурации, настроенных для проекта.</span><span class="sxs-lookup"><span data-stu-id="26a3e-231">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="26a3e-232">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="26a3e-232">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="26a3e-233"><nobr>— Поставщик \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-233"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="26a3e-234">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="26a3e-234">The provider to use.</span></span> <span data-ttu-id="26a3e-235">Обычно это имя пакета NuGet, например: `Microsoft.EntityFrameworkCore.SqlServer` .</span><span class="sxs-lookup"><span data-stu-id="26a3e-235">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="26a3e-236">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="26a3e-236">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="26a3e-237">-OutputDir \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-237">-OutputDir \<String></span></span>               | <span data-ttu-id="26a3e-238">Каталог для размещения файлов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-238">The directory to put files in.</span></span> <span data-ttu-id="26a3e-239">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="26a3e-239">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="26a3e-240">-Контекстдир \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-240">-ContextDir \<String></span></span>              | <span data-ttu-id="26a3e-241">Каталог, в котором следует разместить `DbContext` файл.</span><span class="sxs-lookup"><span data-stu-id="26a3e-241">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="26a3e-242">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="26a3e-242">Paths are relative to the project directory.</span></span>                                                                                                                                                               |
| <span data-ttu-id="26a3e-243">-Namespace \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-243">-Namespace \<String></span></span>               | <span data-ttu-id="26a3e-244">Пространство имен, используемое для всех создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="26a3e-244">The namespace to use for all generated classes.</span></span> <span data-ttu-id="26a3e-245">По умолчанию создается из корневого пространства имен и выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="26a3e-245">Defaults to generated from the root namespace and the output directory.</span></span> <span data-ttu-id="26a3e-246">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-246">Added in EF Core 5.0.</span></span>                                                                                                                           |
| <span data-ttu-id="26a3e-247">-ContextNamespace \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-247">-ContextNamespace \<String></span></span>        | <span data-ttu-id="26a3e-248">Пространство имен, используемое для создаваемого `DbContext` класса.</span><span class="sxs-lookup"><span data-stu-id="26a3e-248">The namespace to use for the generated `DbContext` class.</span></span> <span data-ttu-id="26a3e-249">Примечание. Переопределение `-Namespace` .</span><span class="sxs-lookup"><span data-stu-id="26a3e-249">Note: overrides `-Namespace`.</span></span> <span data-ttu-id="26a3e-250">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-250">Added in EF Core 5.0.</span></span>                                                                                                                                                           |
| <span data-ttu-id="26a3e-251">Контекст \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-251">-Context \<String></span></span>                 | <span data-ttu-id="26a3e-252">Имя `DbContext` создаваемого класса.</span><span class="sxs-lookup"><span data-stu-id="26a3e-252">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="26a3e-253">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="26a3e-253">-Schemas \<String[]></span></span>               | <span data-ttu-id="26a3e-254">Схемы таблиц, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="26a3e-254">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="26a3e-255">Если этот параметр пропущен, включаются все схемы.</span><span class="sxs-lookup"><span data-stu-id="26a3e-255">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="26a3e-256">-Таблицы \<String[]></span><span class="sxs-lookup"><span data-stu-id="26a3e-256">-Tables \<String[]></span></span>                | <span data-ttu-id="26a3e-257">Таблицы, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="26a3e-257">The tables to generate entity types for.</span></span> <span data-ttu-id="26a3e-258">Если этот параметр пропущен, включаются все таблицы.</span><span class="sxs-lookup"><span data-stu-id="26a3e-258">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="26a3e-259">-Аннотации</span><span class="sxs-lookup"><span data-stu-id="26a3e-259">-DataAnnotations</span></span>                   | <span data-ttu-id="26a3e-260">Используйте атрибуты для настройки модели (по возможности).</span><span class="sxs-lookup"><span data-stu-id="26a3e-260">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="26a3e-261">Если этот параметр пропущен, используется только API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="26a3e-261">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="26a3e-262">-Уседатабасенамес</span><span class="sxs-lookup"><span data-stu-id="26a3e-262">-UseDatabaseNames</span></span>                  | <span data-ttu-id="26a3e-263">Имена таблиц и столбцов следует использовать в точности так, как они отображаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-263">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="26a3e-264">Если этот параметр опущен, имена баз данных изменяются в соответствии с соглашениями о стиле имени C#.</span><span class="sxs-lookup"><span data-stu-id="26a3e-264">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="26a3e-265">-Force</span><span class="sxs-lookup"><span data-stu-id="26a3e-265">-Force</span></span>                             | <span data-ttu-id="26a3e-266">Перезаписать существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="26a3e-266">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |
| <span data-ttu-id="26a3e-267">-Нунконфигуринг</span><span class="sxs-lookup"><span data-stu-id="26a3e-267">-NoOnConfiguring</span></span>                   | <span data-ttu-id="26a3e-268">Не создавайте `DbContext.OnConfiguring` .</span><span class="sxs-lookup"><span data-stu-id="26a3e-268">Don't generate `DbContext.OnConfiguring`.</span></span> <span data-ttu-id="26a3e-269">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-269">Added in EF Core 5.0.</span></span>                                                                                                                                                                                                         |
| <span data-ttu-id="26a3e-270">-Ноплурализе</span><span class="sxs-lookup"><span data-stu-id="26a3e-270">-NoPluralize</span></span>                       | <span data-ttu-id="26a3e-271">Не используйте плурализер.</span><span class="sxs-lookup"><span data-stu-id="26a3e-271">Don't use the pluralizer.</span></span> <span data-ttu-id="26a3e-272">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-272">Added in EF Core 5.0.</span></span>                                                                                                                                                                                                                         |

<span data-ttu-id="26a3e-273">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-273">The [common parameters](#common-parameters) are listed above.</span></span>

<span data-ttu-id="26a3e-274">Пример.</span><span class="sxs-lookup"><span data-stu-id="26a3e-274">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="26a3e-275">Пример: формирование шаблонов только для выбранных таблиц и создание контекста в отдельной папке с указанным именем и пространством имен.</span><span class="sxs-lookup"><span data-stu-id="26a3e-275">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name and namespace:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext -ContextNamespace New.Namespace
```

<span data-ttu-id="26a3e-276">В следующем примере считывается строка подключения из конфигурации проекта, которая может быть задана с помощью [средства диспетчера секретов](/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="26a3e-276">The following example reads the connection string from the project's configuration possibly set using the [Secret Manager tool](/aspnet/core/security/app-secrets#secret-manager).</span></span>

```powershell
Scaffold-DbContext "Name=ConnectionStrings:Blogging" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="script-dbcontext"></a><span data-ttu-id="26a3e-277">Script-DbContext</span><span class="sxs-lookup"><span data-stu-id="26a3e-277">Script-DbContext</span></span>

<span data-ttu-id="26a3e-278">Создает скрипт SQL из DbContext.</span><span class="sxs-lookup"><span data-stu-id="26a3e-278">Generates a SQL script from the DbContext.</span></span> <span data-ttu-id="26a3e-279">Обход всех миграций.</span><span class="sxs-lookup"><span data-stu-id="26a3e-279">Bypasses any migrations.</span></span> <span data-ttu-id="26a3e-280">Добавлено в EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-280">Added in EF Core 3.0.</span></span>

<span data-ttu-id="26a3e-281">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-281">Parameters:</span></span>

| <span data-ttu-id="26a3e-282">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-282">Parameter</span></span>                      | <span data-ttu-id="26a3e-283">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-283">Description</span></span>                      |
| ------------------------------ | -------------------------------- |
| <span data-ttu-id="26a3e-284"><nobr>-Output \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-284"><nobr>-Output \<String></nobr></span></span> | <span data-ttu-id="26a3e-285">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="26a3e-285">The file to write the result to.</span></span> |

<span data-ttu-id="26a3e-286">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-286">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="script-migration"></a><span data-ttu-id="26a3e-287">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="26a3e-287">Script-Migration</span></span>

<span data-ttu-id="26a3e-288">Создает скрипт SQL, который применяет все изменения из одной выбранной миграции в другую выбранную миграцию.</span><span class="sxs-lookup"><span data-stu-id="26a3e-288">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="26a3e-289">Параметры</span><span class="sxs-lookup"><span data-stu-id="26a3e-289">Parameters:</span></span>

| <span data-ttu-id="26a3e-290">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-290">Parameter</span></span>                    | <span data-ttu-id="26a3e-291">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-291">Description</span></span>                                                                                                                                                                                                                |
|:---------------------------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26a3e-292">*— От*\<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-292">*-From* \<String></span></span>            | <span data-ttu-id="26a3e-293">Начало миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-293">The starting migration.</span></span> <span data-ttu-id="26a3e-294">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="26a3e-294">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="26a3e-295">Число 0 — это особый случай, который означает *перед первой миграцией*.</span><span class="sxs-lookup"><span data-stu-id="26a3e-295">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="26a3e-296">Значение по умолчанию — 0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-296">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="26a3e-297">*— Для*\<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-297">*-To* \<String></span></span>              | <span data-ttu-id="26a3e-298">Завершение миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-298">The ending migration.</span></span> <span data-ttu-id="26a3e-299">По умолчанию используется последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="26a3e-299">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="26a3e-300">-Идемпотентными</span><span class="sxs-lookup"><span data-stu-id="26a3e-300">-Idempotent</span></span>                  | <span data-ttu-id="26a3e-301">Создание скрипта, который может использоваться в базе данных при любой миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-301">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="26a3e-302"><nobr>-Нетранзакций</nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-302"><nobr>-NoTransactions</nobr></span></span> | <span data-ttu-id="26a3e-303">Не создавайте инструкции SQL Transaction.</span><span class="sxs-lookup"><span data-stu-id="26a3e-303">Don't generate SQL transaction statements.</span></span> <span data-ttu-id="26a3e-304">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-304">Added in EF Core 5.0.</span></span>                                                                                                                                                           |
| <span data-ttu-id="26a3e-305">-Output \<String></span><span class="sxs-lookup"><span data-stu-id="26a3e-305">-Output \<String></span></span>            | <span data-ttu-id="26a3e-306">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="26a3e-306">The file to write the result to.</span></span> <span data-ttu-id="26a3e-307">Если этот параметр не указан, файл создается с именем, созданным в той же папке, что и файлы среды выполнения приложения, например: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/*.</span><span class="sxs-lookup"><span data-stu-id="26a3e-307">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

<span data-ttu-id="26a3e-308">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-308">The [common parameters](#common-parameters) are listed above.</span></span>

> [!TIP]
> <span data-ttu-id="26a3e-309">Параметры to, from и Output поддерживают расширение клавишей TAB.</span><span class="sxs-lookup"><span data-stu-id="26a3e-309">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="26a3e-310">В следующем примере создается скрипт для миграции InitialCreate (из базы данных без каких-либо миграций) с использованием имени миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-310">The following example creates a script for the InitialCreate migration (from a database without any migrations), using the migration name.</span></span>

```powershell
Script-Migration 0 InitialCreate
```

<span data-ttu-id="26a3e-311">В следующем примере создается скрипт для всех миграций после миграции InitialCreate с использованием идентификатора миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-311">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="26a3e-312">Update-Database</span><span class="sxs-lookup"><span data-stu-id="26a3e-312">Update-Database</span></span>

<span data-ttu-id="26a3e-313">Обновляет базу данных до последней миграции или до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-313">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="26a3e-314">Параметр</span><span class="sxs-lookup"><span data-stu-id="26a3e-314">Parameter</span></span>                           | <span data-ttu-id="26a3e-315">Описание</span><span class="sxs-lookup"><span data-stu-id="26a3e-315">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26a3e-316"><nobr>*-Миграция*\<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-316"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="26a3e-317">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="26a3e-317">The target migration.</span></span> <span data-ttu-id="26a3e-318">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="26a3e-318">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="26a3e-319">Число 0 — это особый случай, который означает *перед первой миграцией* и приводит к отмене всех миграций.</span><span class="sxs-lookup"><span data-stu-id="26a3e-319">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="26a3e-320">Если миграция не указана, команда по умолчанию принимает значение последней миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-320">If no migration is specified, the command defaults to the last migration.</span></span> |
| <span data-ttu-id="26a3e-321"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="26a3e-321"><nobr>-Connection \<String></nobr></span></span>  | <span data-ttu-id="26a3e-322">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="26a3e-322">The connection string to the database.</span></span> <span data-ttu-id="26a3e-323">По умолчанию используется значение, заданное в `AddDbContext` или `OnConfiguring` .</span><span class="sxs-lookup"><span data-stu-id="26a3e-323">Defaults to the one specified in `AddDbContext` or `OnConfiguring`.</span></span> <span data-ttu-id="26a3e-324">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="26a3e-324">Added in EF Core 5.0.</span></span>                                                                                                                                |

<span data-ttu-id="26a3e-325">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="26a3e-325">The [common parameters](#common-parameters) are listed above.</span></span>

> [!TIP]
> <span data-ttu-id="26a3e-326">Параметр миграции поддерживает расширение клавиш TAB.</span><span class="sxs-lookup"><span data-stu-id="26a3e-326">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="26a3e-327">В следующем примере возвращаются все миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-327">The following example reverts all migrations.</span></span>

```powershell
Update-Database 0
```

<span data-ttu-id="26a3e-328">В следующих примерах база данных обновляется до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="26a3e-328">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="26a3e-329">В первом случае используется имя миграции, а во втором используется идентификатор миграции и указанное соединение:</span><span class="sxs-lookup"><span data-stu-id="26a3e-329">The first uses the migration name and the second uses the migration ID and a specified connection:</span></span>

```powershell
Update-Database InitialCreate
Update-Database 20180904195021_InitialCreate -Connection your_connection_string
```

## <a name="additional-resources"></a><span data-ttu-id="26a3e-330">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="26a3e-330">Additional resources</span></span>

* [<span data-ttu-id="26a3e-331">Миграции</span><span class="sxs-lookup"><span data-stu-id="26a3e-331">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="26a3e-332">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="26a3e-332">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
