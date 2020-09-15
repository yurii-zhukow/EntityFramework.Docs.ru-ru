---
title: Справочник по средствам EF Core (консоль диспетчера пакетов) — EF Core
description: Справочное руководство по консоли диспетчера пакетов Visual Studio Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 09/09/2020
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 5dca397978c60c12610d9080caba972a66b079b6
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071866"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="27edc-103">Справочник по средствам Entity Framework Core. консоль диспетчера пакетов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27edc-103">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="27edc-104">Средства консоли диспетчера пакетов (PMC) для Entity Framework Core выполнения задач разработки во время разработки.</span><span class="sxs-lookup"><span data-stu-id="27edc-104">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="27edc-105">Например, они создают [миграции](/aspnet/core/data/ef-mvc/migrations), применяют миграции и создают код для модели на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-105">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="27edc-106">Команды выполняются в Visual Studio с помощью [консоли диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="27edc-106">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="27edc-107">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="27edc-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="27edc-108">Если вы не используете Visual Studio, вместо этого рекомендуется использовать [средства командной строки EF Core](xref:core/miscellaneous/cli/dotnet) .</span><span class="sxs-lookup"><span data-stu-id="27edc-108">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](xref:core/miscellaneous/cli/dotnet) instead.</span></span> <span data-ttu-id="27edc-109">Средства .NET Core CLI являются кросс-платформенными и выполняются в командной строке.</span><span class="sxs-lookup"><span data-stu-id="27edc-109">The .NET Core CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="27edc-110">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="27edc-110">Installing the tools</span></span>

<span data-ttu-id="27edc-111">Процедуры установки и обновления инструментов отличаются от ASP.NET Core 2.1 + и более ранних версий или других типов проектов.</span><span class="sxs-lookup"><span data-stu-id="27edc-111">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="27edc-112">ASP.NET Core версии 2,1 и более поздних</span><span class="sxs-lookup"><span data-stu-id="27edc-112">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="27edc-113">Эти средства автоматически включаются в проект ASP.NET Core 2.1 +, так как `Microsoft.EntityFrameworkCore.Tools` пакет входит в состав [метапакет Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="27edc-113">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="27edc-114">Поэтому вам не нужно ничего делать для установки средств, но вам нужно:</span><span class="sxs-lookup"><span data-stu-id="27edc-114">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="27edc-115">Восстановите пакеты перед использованием средств в новом проекте.</span><span class="sxs-lookup"><span data-stu-id="27edc-115">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="27edc-116">Установите пакет, чтобы обновить средства до более новой версии.</span><span class="sxs-lookup"><span data-stu-id="27edc-116">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="27edc-117">Чтобы убедиться, что вы получаете последнюю версию средств, мы рекомендуем также выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="27edc-117">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="27edc-118">Измените *CSPROJ* -файл и добавьте строку, указывающую последнюю версию пакета [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="27edc-118">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="27edc-119">Например, файл *. csproj* может включать в себя следующий `ItemGroup` вид:</span><span class="sxs-lookup"><span data-stu-id="27edc-119">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="3.1.2" />
  </ItemGroup>
  ```

<span data-ttu-id="27edc-120">Обновите средства при получении сообщения, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="27edc-120">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="27edc-121">Версия инструментов EF Core "короче-RTM-30846" старше, чем версия среды выполнения "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="27edc-121">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="27edc-122">Обновите средства для получения последних компонентов и исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="27edc-122">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="27edc-123">Чтобы обновить средства, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="27edc-123">To update the tools:</span></span>

* <span data-ttu-id="27edc-124">Установите пакет SDK для .NET Core последней версии.</span><span class="sxs-lookup"><span data-stu-id="27edc-124">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="27edc-125">Обновите Visual Studio до последней версии.</span><span class="sxs-lookup"><span data-stu-id="27edc-125">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="27edc-126">Измените *CSPROJ* -файл, чтобы он включал ссылку на пакет для последней версии пакета инструментов, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-126">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="27edc-127">Другие версии и типы проектов</span><span class="sxs-lookup"><span data-stu-id="27edc-127">Other versions and project types</span></span>

<span data-ttu-id="27edc-128">Установите средства консоли диспетчера пакетов, выполнив следующую команду в **консоли диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="27edc-128">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="27edc-129">Обновите средства, выполнив следующую команду в **консоли диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="27edc-129">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="27edc-130">Проверка установки</span><span class="sxs-lookup"><span data-stu-id="27edc-130">Verify the installation</span></span>

<span data-ttu-id="27edc-131">Убедитесь, что средства установлены, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="27edc-131">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="27edc-132">Выходные данные выглядят следующим образом (он не указывает, какую версию инструментов вы используете):</span><span class="sxs-lookup"><span data-stu-id="27edc-132">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

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

## <a name="using-the-tools"></a><span data-ttu-id="27edc-133">Использование средств</span><span class="sxs-lookup"><span data-stu-id="27edc-133">Using the tools</span></span>

<span data-ttu-id="27edc-134">Перед использованием средств:</span><span class="sxs-lookup"><span data-stu-id="27edc-134">Before using the tools:</span></span>

* <span data-ttu-id="27edc-135">Изучите разницу между целевым и запускаемым проектами.</span><span class="sxs-lookup"><span data-stu-id="27edc-135">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="27edc-136">Узнайте, как использовать средства с библиотеками классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="27edc-136">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="27edc-137">Для проектов ASP.NET Core задайте среду.</span><span class="sxs-lookup"><span data-stu-id="27edc-137">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="27edc-138">Целевой и запускаемый проекты</span><span class="sxs-lookup"><span data-stu-id="27edc-138">Target and startup project</span></span>

<span data-ttu-id="27edc-139">Команды ссылаются на *проект* и *запускаемый проект*.</span><span class="sxs-lookup"><span data-stu-id="27edc-139">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="27edc-140">*Проект* также называется *целевым проектом* , так как он служит для добавления или удаления файлов.</span><span class="sxs-lookup"><span data-stu-id="27edc-140">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="27edc-141">По умолчанию **проект** , выбранный в **консоли диспетчера пакетов** , является целевым.</span><span class="sxs-lookup"><span data-stu-id="27edc-141">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="27edc-142">Можно указать другой проект в качестве целевого проекта с помощью <nobr>`--project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="27edc-142">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="27edc-143">*Запускаемый проект* — это тот, который выполняет сборка и запуск средств.</span><span class="sxs-lookup"><span data-stu-id="27edc-143">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="27edc-144">Средства должны выполнять код приложения во время разработки для получения сведений о проекте, таких как строка подключения к базе данных и Конфигурация модели.</span><span class="sxs-lookup"><span data-stu-id="27edc-144">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="27edc-145">По умолчанию **запускаемым проектом** в **Обозреватель решений** является запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="27edc-145">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="27edc-146">Можно указать другой проект в качестве запускаемого проекта с помощью <nobr>`--startup-project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="27edc-146">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="27edc-147">Запускаемый проект и целевой проект часто являются одним и тем же проектом.</span><span class="sxs-lookup"><span data-stu-id="27edc-147">The startup project and target project are often the same project.</span></span> <span data-ttu-id="27edc-148">Типичный сценарий, в котором они являются отдельными проектами, состоит в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="27edc-148">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="27edc-149">Контекст EF Core и классы сущностей находятся в библиотеке классов .NET Core.</span><span class="sxs-lookup"><span data-stu-id="27edc-149">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="27edc-150">Консольное приложение .NET Core или веб-приложение ссылается на библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="27edc-150">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="27edc-151">Также можно [разместить код миграции в библиотеке классов, отдельном от контекста EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="27edc-151">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="27edc-152">Другие целевые платформы</span><span class="sxs-lookup"><span data-stu-id="27edc-152">Other target frameworks</span></span>

<span data-ttu-id="27edc-153">Средства консоли диспетчера пакетов работают с проектами .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="27edc-153">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="27edc-154">Приложения с моделью EF Core в библиотеке классов .NET Standard могут не иметь проекта .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="27edc-154">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="27edc-155">Например, это справедливо для приложений Xamarin и универсальная платформа Windows.</span><span class="sxs-lookup"><span data-stu-id="27edc-155">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="27edc-156">В таких случаях можно создать проект консольного приложения .NET Core или .NET Framework, предназначенное только для запуска в качестве запускаемого проекта для инструментов.</span><span class="sxs-lookup"><span data-stu-id="27edc-156">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="27edc-157">Проект может быть фиктивным проектом без реального кода, &mdash; поэтому он необходим только для предоставления целей для инструментов.</span><span class="sxs-lookup"><span data-stu-id="27edc-157">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="27edc-158">Зачем нужен фиктивный проект?</span><span class="sxs-lookup"><span data-stu-id="27edc-158">Why is a dummy project required?</span></span> <span data-ttu-id="27edc-159">Как упоминалось ранее, средства должны выполнять код приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="27edc-159">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="27edc-160">Для этого необходимо использовать среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="27edc-160">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="27edc-161">Если EF Coreная модель находится в проекте, ориентированном на .NET Core или .NET Framework, EF Core средства позаимствованы среду выполнения из проекта.</span><span class="sxs-lookup"><span data-stu-id="27edc-161">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="27edc-162">Они не могут сделать это, если модель EF Core находится в библиотеке классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="27edc-162">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="27edc-163">.NET Standard не является реальной реализацией .NET; это спецификация набора интерфейсов API, которые должны поддерживаться реализациями .NET.</span><span class="sxs-lookup"><span data-stu-id="27edc-163">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="27edc-164">Поэтому .NET Standard недостаточно для того, чтобы средства EF Core выполняли код приложения.</span><span class="sxs-lookup"><span data-stu-id="27edc-164">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="27edc-165">Фиктивный проект, который создается для использования в качестве запускаемого проекта, предоставляет конкретную целевую платформу, в которую средства могут загрузить библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="27edc-165">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="27edc-166">Среда ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27edc-166">ASP.NET Core environment</span></span>

<span data-ttu-id="27edc-167">Чтобы указать среду для проектов ASP.NET Core, установите параметр **env: ASPNETCORE_ENVIRONMENT** перед выполнением команд.</span><span class="sxs-lookup"><span data-stu-id="27edc-167">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="27edc-168">Общие параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-168">Common parameters</span></span>

<span data-ttu-id="27edc-169">В следующей таблице приведены параметры, которые являются общими для всех EF Coreных команд.</span><span class="sxs-lookup"><span data-stu-id="27edc-169">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="27edc-170">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-170">Parameter</span></span>                 | <span data-ttu-id="27edc-171">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-171">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="27edc-172">Контекст \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-172">-Context \<String></span></span>        | <span data-ttu-id="27edc-173">Класс `DbContext` для использования.</span><span class="sxs-lookup"><span data-stu-id="27edc-173">The `DbContext` class to use.</span></span> <span data-ttu-id="27edc-174">Только имя класса или полный квалификатор с пространствами имен.</span><span class="sxs-lookup"><span data-stu-id="27edc-174">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="27edc-175">Если этот параметр опущен, EF Core находит класс контекста.</span><span class="sxs-lookup"><span data-stu-id="27edc-175">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="27edc-176">Если имеется несколько классов контекста, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="27edc-176">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="27edc-177">-Проект \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-177">-Project \<String></span></span>        | <span data-ttu-id="27edc-178">Целевой проект.</span><span class="sxs-lookup"><span data-stu-id="27edc-178">The target project.</span></span> <span data-ttu-id="27edc-179">Если этот параметр не указан, в качестве целевого проекта используется **проект по умолчанию** для **консоли диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="27edc-179">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="27edc-180"><nobr>-Стартуппрожект</nobr>\<String></span><span class="sxs-lookup"><span data-stu-id="27edc-180"><nobr>-StartupProject</nobr> \<String></span></span> | <span data-ttu-id="27edc-181">Автоматически запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="27edc-181">The startup project.</span></span> <span data-ttu-id="27edc-182">Если этот параметр не указан, в качестве целевого проекта используется **запускаемый проект** в **свойствах решения** .</span><span class="sxs-lookup"><span data-stu-id="27edc-182">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="27edc-183">-Args \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-183">-Args \<String></span></span>           | <span data-ttu-id="27edc-184">Аргументы, передаваемые в приложение.</span><span class="sxs-lookup"><span data-stu-id="27edc-184">Arguments passed to the application.</span></span> <span data-ttu-id="27edc-185">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-185">Added in EF Core 5.0.</span></span>                                                                                                                                                           |
| <span data-ttu-id="27edc-186">-Verbose</span><span class="sxs-lookup"><span data-stu-id="27edc-186">-Verbose</span></span>                  | <span data-ttu-id="27edc-187">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-187">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="27edc-188">Чтобы отобразить справочные сведения о команде, используйте команду PowerShell `Get-Help` .</span><span class="sxs-lookup"><span data-stu-id="27edc-188">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="27edc-189">Параметры Context, Project и Стартуппрожект поддерживают расширение Tab.</span><span class="sxs-lookup"><span data-stu-id="27edc-189">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="27edc-190">Добавление и миграция</span><span class="sxs-lookup"><span data-stu-id="27edc-190">Add-Migration</span></span>

<span data-ttu-id="27edc-191">Добавляет новый перенос.</span><span class="sxs-lookup"><span data-stu-id="27edc-191">Adds a new migration.</span></span>

<span data-ttu-id="27edc-192">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-192">Parameters:</span></span>

| <span data-ttu-id="27edc-193">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-193">Parameter</span></span>                         | <span data-ttu-id="27edc-194">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-194">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="27edc-195"><nobr>-Имя \<String><nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-195"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="27edc-196">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-196">The name of the migration.</span></span> <span data-ttu-id="27edc-197">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="27edc-197">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="27edc-198"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-198"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="27edc-199">Каталог используется для вывода файлов.</span><span class="sxs-lookup"><span data-stu-id="27edc-199">The directory use to output the files.</span></span> <span data-ttu-id="27edc-200">Пути задаются относительно целевого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="27edc-200">Paths are relative to the target project directory.</span></span> <span data-ttu-id="27edc-201">По умолчанию используется значение "миграции".</span><span class="sxs-lookup"><span data-stu-id="27edc-201">Defaults to "Migrations".</span></span> |
| <span data-ttu-id="27edc-202"><nobr>-Namespace \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-202"><nobr>-Namespace \<String></nobr></span></span> | <span data-ttu-id="27edc-203">Пространство имен, используемое для создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="27edc-203">The namespace to use for the generated classes.</span></span> <span data-ttu-id="27edc-204">По умолчанию создается из выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="27edc-204">Defaults to generated from the output directory.</span></span> <span data-ttu-id="27edc-205">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-205">Added in EF Core 5.0.</span></span>  |

<span data-ttu-id="27edc-206">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-206">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="drop-database"></a><span data-ttu-id="27edc-207">Удалить базу данных</span><span class="sxs-lookup"><span data-stu-id="27edc-207">Drop-Database</span></span>

<span data-ttu-id="27edc-208">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-208">Drops the database.</span></span>

<span data-ttu-id="27edc-209">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-209">Parameters:</span></span>

| <span data-ttu-id="27edc-210">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-210">Parameter</span></span> | <span data-ttu-id="27edc-211">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-211">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="27edc-212">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="27edc-212">-WhatIf</span></span>   | <span data-ttu-id="27edc-213">Показывает, какую базу данных следует удалить, но не удалять.</span><span class="sxs-lookup"><span data-stu-id="27edc-213">Show which database would be dropped, but don't drop it.</span></span> |

<span data-ttu-id="27edc-214">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-214">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="get-dbcontext"></a><span data-ttu-id="27edc-215">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="27edc-215">Get-DbContext</span></span>

<span data-ttu-id="27edc-216">Перечисляет и получает сведения о доступных `DbContext` типах.</span><span class="sxs-lookup"><span data-stu-id="27edc-216">Lists and gets information about available `DbContext` types.</span></span>

<span data-ttu-id="27edc-217">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-217">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="get-migration"></a><span data-ttu-id="27edc-218">Получение и миграция</span><span class="sxs-lookup"><span data-stu-id="27edc-218">Get-Migration</span></span>

<span data-ttu-id="27edc-219">Список доступных миграций.</span><span class="sxs-lookup"><span data-stu-id="27edc-219">Lists available migrations.</span></span> <span data-ttu-id="27edc-220">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-220">Added in EF Core 5.0.</span></span>

<span data-ttu-id="27edc-221">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-221">Parameters:</span></span>

| <span data-ttu-id="27edc-222">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-222">Parameter</span></span>                          | <span data-ttu-id="27edc-223">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-223">Description</span></span>                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------ |
| <span data-ttu-id="27edc-224"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-224"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="27edc-225">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-225">The connection string to the database.</span></span> <span data-ttu-id="27edc-226">По умолчанию используется значение, указанное в AddDbContext или onconfiguring.</span><span class="sxs-lookup"><span data-stu-id="27edc-226">Defaults to the one specified in AddDbContext or OnConfiguring.</span></span> |
| <span data-ttu-id="27edc-227">-Соединение</span><span class="sxs-lookup"><span data-stu-id="27edc-227">-NoConnect</span></span>                         | <span data-ttu-id="27edc-228">Не подключайтесь к базе данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-228">Don't connect to the database.</span></span>                                                                         |

<span data-ttu-id="27edc-229">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-229">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="27edc-230">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="27edc-230">Remove-Migration</span></span>

<span data-ttu-id="27edc-231">Удаляет последнюю миграцию (выполняет откат изменений кода, выполненных для миграции).</span><span class="sxs-lookup"><span data-stu-id="27edc-231">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="27edc-232">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-232">Parameters:</span></span>

| <span data-ttu-id="27edc-233">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-233">Parameter</span></span> | <span data-ttu-id="27edc-234">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-234">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="27edc-235">-Force</span><span class="sxs-lookup"><span data-stu-id="27edc-235">-Force</span></span>    | <span data-ttu-id="27edc-236">Отмените миграцию (выполните откат изменений, примененных к базе данных).</span><span class="sxs-lookup"><span data-stu-id="27edc-236">Revert the migration (roll back the changes that were applied to the database).</span></span> |

<span data-ttu-id="27edc-237">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-237">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="scaffold-dbcontext"></a><span data-ttu-id="27edc-238">Формирование шаблонов — DbContext</span><span class="sxs-lookup"><span data-stu-id="27edc-238">Scaffold-DbContext</span></span>

<span data-ttu-id="27edc-239">Создает код для `DbContext` типов сущностей и для базы данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-239">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="27edc-240">`Scaffold-DbContext`Чтобы создать тип сущности, таблица базы данных должна иметь первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="27edc-240">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="27edc-241">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-241">Parameters:</span></span>

| <span data-ttu-id="27edc-242">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-242">Parameter</span></span>                          | <span data-ttu-id="27edc-243">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-243">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="27edc-244"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-244"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="27edc-245">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-245">The connection string to the database.</span></span> <span data-ttu-id="27edc-246">Для проектов ASP.NET Core 2. x значением может быть \*Name = \<name of connection string> \*.</span><span class="sxs-lookup"><span data-stu-id="27edc-246">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="27edc-247">В этом случае имя берется из источников конфигурации, настроенных для проекта.</span><span class="sxs-lookup"><span data-stu-id="27edc-247">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="27edc-248">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="27edc-248">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="27edc-249"><nobr>— Поставщик \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-249"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="27edc-250">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="27edc-250">The provider to use.</span></span> <span data-ttu-id="27edc-251">Обычно это имя пакета NuGet, например: `Microsoft.EntityFrameworkCore.SqlServer` .</span><span class="sxs-lookup"><span data-stu-id="27edc-251">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="27edc-252">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="27edc-252">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="27edc-253">-OutputDir \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-253">-OutputDir \<String></span></span>               | <span data-ttu-id="27edc-254">Каталог для размещения файлов.</span><span class="sxs-lookup"><span data-stu-id="27edc-254">The directory to put files in.</span></span> <span data-ttu-id="27edc-255">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="27edc-255">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="27edc-256">-Контекстдир \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-256">-ContextDir \<String></span></span>              | <span data-ttu-id="27edc-257">Каталог, в котором следует разместить `DbContext` файл.</span><span class="sxs-lookup"><span data-stu-id="27edc-257">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="27edc-258">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="27edc-258">Paths are relative to the project directory.</span></span>                                                                                                                                                               |
| <span data-ttu-id="27edc-259">-Namespace \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-259">-Namespace \<String></span></span>               | <span data-ttu-id="27edc-260">Пространство имен, используемое для всех создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="27edc-260">The namespace to use for all generated classes.</span></span> <span data-ttu-id="27edc-261">По умолчанию создается из корневого пространства имен и выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="27edc-261">Defaults to generated from the root namespace and the output directory.</span></span> <span data-ttu-id="27edc-262">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-262">Added in EF Core 5.0.</span></span>                                                                                                                           |
| <span data-ttu-id="27edc-263">-ContextNamespace \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-263">-ContextNamespace \<String></span></span>        | <span data-ttu-id="27edc-264">Пространство имен, используемое для создаваемого `DbContext` класса.</span><span class="sxs-lookup"><span data-stu-id="27edc-264">The namespace to use for the generated `DbContext` class.</span></span> <span data-ttu-id="27edc-265">Примечание. Переопределение `-Namespace` .</span><span class="sxs-lookup"><span data-stu-id="27edc-265">Note: overrides `-Namespace`.</span></span> <span data-ttu-id="27edc-266">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-266">Added in EF Core 5.0.</span></span>                                                                                                                                                           |
| <span data-ttu-id="27edc-267">Контекст \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-267">-Context \<String></span></span>                 | <span data-ttu-id="27edc-268">Имя `DbContext` создаваемого класса.</span><span class="sxs-lookup"><span data-stu-id="27edc-268">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="27edc-269">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="27edc-269">-Schemas \<String[]></span></span>               | <span data-ttu-id="27edc-270">Схемы таблиц, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="27edc-270">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="27edc-271">Если этот параметр пропущен, включаются все схемы.</span><span class="sxs-lookup"><span data-stu-id="27edc-271">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="27edc-272">-Таблицы \<String[]></span><span class="sxs-lookup"><span data-stu-id="27edc-272">-Tables \<String[]></span></span>                | <span data-ttu-id="27edc-273">Таблицы, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="27edc-273">The tables to generate entity types for.</span></span> <span data-ttu-id="27edc-274">Если этот параметр пропущен, включаются все таблицы.</span><span class="sxs-lookup"><span data-stu-id="27edc-274">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="27edc-275">-Аннотации</span><span class="sxs-lookup"><span data-stu-id="27edc-275">-DataAnnotations</span></span>                   | <span data-ttu-id="27edc-276">Используйте атрибуты для настройки модели (по возможности).</span><span class="sxs-lookup"><span data-stu-id="27edc-276">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="27edc-277">Если этот параметр пропущен, используется только API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="27edc-277">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="27edc-278">-Уседатабасенамес</span><span class="sxs-lookup"><span data-stu-id="27edc-278">-UseDatabaseNames</span></span>                  | <span data-ttu-id="27edc-279">Имена таблиц и столбцов следует использовать в точности так, как они отображаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-279">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="27edc-280">Если этот параметр опущен, имена баз данных изменяются в соответствии с соглашениями о стиле имени C#.</span><span class="sxs-lookup"><span data-stu-id="27edc-280">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="27edc-281">-Force</span><span class="sxs-lookup"><span data-stu-id="27edc-281">-Force</span></span>                             | <span data-ttu-id="27edc-282">Перезаписать существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="27edc-282">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |
| <span data-ttu-id="27edc-283">-Нунконфигуринг</span><span class="sxs-lookup"><span data-stu-id="27edc-283">-NoOnConfiguring</span></span>                   | <span data-ttu-id="27edc-284">Не создавайте `DbContext.OnConfiguring` .</span><span class="sxs-lookup"><span data-stu-id="27edc-284">Don't generate `DbContext.OnConfiguring`.</span></span> <span data-ttu-id="27edc-285">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-285">Added in EF Core 5.0.</span></span>                                                                                                                                                                                                         |
| <span data-ttu-id="27edc-286">-Ноплурализе</span><span class="sxs-lookup"><span data-stu-id="27edc-286">-NoPluralize</span></span>                       | <span data-ttu-id="27edc-287">Не используйте плурализер.</span><span class="sxs-lookup"><span data-stu-id="27edc-287">Don't use the pluralizer.</span></span> <span data-ttu-id="27edc-288">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-288">Added in EF Core 5.0.</span></span>                                                                                                                                                                                                                         |

<span data-ttu-id="27edc-289">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-289">The [common parameters](#common-parameters) are listed above.</span></span>

<span data-ttu-id="27edc-290">Пример:</span><span class="sxs-lookup"><span data-stu-id="27edc-290">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="27edc-291">Пример: формирование шаблонов только для выбранных таблиц и создание контекста в отдельной папке с указанным именем и пространством имен.</span><span class="sxs-lookup"><span data-stu-id="27edc-291">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name and namespace:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext -ContextNamespace New.Namespace
```

## <a name="script-dbcontext"></a><span data-ttu-id="27edc-292">Сценарий — DbContext</span><span class="sxs-lookup"><span data-stu-id="27edc-292">Script-DbContext</span></span>

<span data-ttu-id="27edc-293">Создает скрипт SQL из DbContext.</span><span class="sxs-lookup"><span data-stu-id="27edc-293">Generates a SQL script from the DbContext.</span></span> <span data-ttu-id="27edc-294">Обход всех миграций.</span><span class="sxs-lookup"><span data-stu-id="27edc-294">Bypasses any migrations.</span></span> <span data-ttu-id="27edc-295">Добавлено в EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-295">Added in EF Core 3.0.</span></span>

<span data-ttu-id="27edc-296">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-296">Parameters:</span></span>

| <span data-ttu-id="27edc-297">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-297">Parameter</span></span>                      | <span data-ttu-id="27edc-298">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-298">Description</span></span>                      |
| ------------------------------ | -------------------------------- |
| <span data-ttu-id="27edc-299"><nobr>-Output \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-299"><nobr>-Output \<String></nobr></span></span> | <span data-ttu-id="27edc-300">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="27edc-300">The file to write the result to.</span></span> |

<span data-ttu-id="27edc-301">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-301">The [common parameters](#common-parameters) are listed above.</span></span>

## <a name="script-migration"></a><span data-ttu-id="27edc-302">Скрипт — миграция</span><span class="sxs-lookup"><span data-stu-id="27edc-302">Script-Migration</span></span>

<span data-ttu-id="27edc-303">Создает скрипт SQL, который применяет все изменения из одной выбранной миграции в другую выбранную миграцию.</span><span class="sxs-lookup"><span data-stu-id="27edc-303">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="27edc-304">Параметры</span><span class="sxs-lookup"><span data-stu-id="27edc-304">Parameters:</span></span>

| <span data-ttu-id="27edc-305">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-305">Parameter</span></span>                    | <span data-ttu-id="27edc-306">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-306">Description</span></span>                                                                                                                                                                                                                |
|:---------------------------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="27edc-307">*— От*\<String></span><span class="sxs-lookup"><span data-stu-id="27edc-307">*-From* \<String></span></span>            | <span data-ttu-id="27edc-308">Начало миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-308">The starting migration.</span></span> <span data-ttu-id="27edc-309">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="27edc-309">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="27edc-310">Число 0 — это особый случай, который означает *перед первой миграцией*.</span><span class="sxs-lookup"><span data-stu-id="27edc-310">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="27edc-311">Значение по умолчанию — 0.</span><span class="sxs-lookup"><span data-stu-id="27edc-311">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="27edc-312">*— Для*\<String></span><span class="sxs-lookup"><span data-stu-id="27edc-312">*-To* \<String></span></span>              | <span data-ttu-id="27edc-313">Завершение миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-313">The ending migration.</span></span> <span data-ttu-id="27edc-314">По умолчанию используется последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="27edc-314">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="27edc-315">-Идемпотентными</span><span class="sxs-lookup"><span data-stu-id="27edc-315">-Idempotent</span></span>                  | <span data-ttu-id="27edc-316">Создание скрипта, который может использоваться в базе данных при любой миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-316">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="27edc-317"><nobr>-Нетранзакций</nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-317"><nobr>-NoTransactions</nobr></span></span> | <span data-ttu-id="27edc-318">Не создавайте инструкции SQL Transaction.</span><span class="sxs-lookup"><span data-stu-id="27edc-318">Don't generate SQL transaction statements.</span></span> <span data-ttu-id="27edc-319">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-319">Added in EF Core 5.0.</span></span>                                                                                                                                                           |
| <span data-ttu-id="27edc-320">-Output \<String></span><span class="sxs-lookup"><span data-stu-id="27edc-320">-Output \<String></span></span>            | <span data-ttu-id="27edc-321">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="27edc-321">The file to write the result to.</span></span> <span data-ttu-id="27edc-322">Если этот параметр не указан, файл создается с именем, созданным в той же папке, что и файлы среды выполнения приложения, например: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/*.</span><span class="sxs-lookup"><span data-stu-id="27edc-322">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

<span data-ttu-id="27edc-323">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-323">The [common parameters](#common-parameters) are listed above.</span></span>

> [!TIP]
> <span data-ttu-id="27edc-324">Параметры to, from и Output поддерживают расширение клавишей TAB.</span><span class="sxs-lookup"><span data-stu-id="27edc-324">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="27edc-325">В следующем примере создается скрипт для миграции InitialCreate с использованием имени миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-325">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="27edc-326">В следующем примере создается скрипт для всех миграций после миграции InitialCreate с использованием идентификатора миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-326">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="27edc-327">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="27edc-327">Update-Database</span></span>

<span data-ttu-id="27edc-328">Обновляет базу данных до последней миграции или до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-328">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="27edc-329">Параметр</span><span class="sxs-lookup"><span data-stu-id="27edc-329">Parameter</span></span>                           | <span data-ttu-id="27edc-330">Описание</span><span class="sxs-lookup"><span data-stu-id="27edc-330">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="27edc-331"><nobr>*-Миграция*\<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-331"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="27edc-332">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="27edc-332">The target migration.</span></span> <span data-ttu-id="27edc-333">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="27edc-333">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="27edc-334">Число 0 — это особый случай, который означает *перед первой миграцией* и приводит к отмене всех миграций.</span><span class="sxs-lookup"><span data-stu-id="27edc-334">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="27edc-335">Если миграция не указана, команда по умолчанию принимает значение последней миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-335">If no migration is specified, the command defaults to the last migration.</span></span> |
| <span data-ttu-id="27edc-336"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="27edc-336"><nobr>-Connection \<String></nobr></span></span>  | <span data-ttu-id="27edc-337">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="27edc-337">The connection string to the database.</span></span> <span data-ttu-id="27edc-338">По умолчанию используется значение, заданное в `AddDbContext` или `OnConfiguring` .</span><span class="sxs-lookup"><span data-stu-id="27edc-338">Defaults to the one specified in `AddDbContext` or `OnConfiguring`.</span></span> <span data-ttu-id="27edc-339">Добавлено в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="27edc-339">Added in EF Core 5.0.</span></span>                                                                                                                                |

<span data-ttu-id="27edc-340">[Общие параметры](#common-parameters) перечислены выше.</span><span class="sxs-lookup"><span data-stu-id="27edc-340">The [common parameters](#common-parameters) are listed above.</span></span>

> [!TIP]
> <span data-ttu-id="27edc-341">Параметр миграции поддерживает расширение клавиш TAB.</span><span class="sxs-lookup"><span data-stu-id="27edc-341">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="27edc-342">В следующем примере возвращаются все миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-342">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="27edc-343">В следующих примерах база данных обновляется до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="27edc-343">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="27edc-344">В первом случае используется имя миграции, а во втором используется идентификатор миграции и указанное соединение:</span><span class="sxs-lookup"><span data-stu-id="27edc-344">The first uses the migration name and the second uses the migration ID and a specified connection:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate -Connection your_connection_string
```

## <a name="additional-resources"></a><span data-ttu-id="27edc-345">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="27edc-345">Additional resources</span></span>

* [<span data-ttu-id="27edc-346">Миграции</span><span class="sxs-lookup"><span data-stu-id="27edc-346">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="27edc-347">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="27edc-347">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
